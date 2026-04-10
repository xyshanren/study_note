> 学习日期：2026-04-30（周三）

## 学习目标

1. 掌握 JWT 认证实现
2. 理解密码加密
3. 学会 API 安全防护

---

## 1. 密码安全

### 1.1 安装依赖

```bash
pip install passlib[bcrypt] python-jose python-multipart
```

### 1.2 密码哈希

```python
# utils/password.py
from passlib.context import CryptContext

# 配置 bcrypt 加密
pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """验证密码"""
    return pwd_context.verify(plain_password, hashed_password)

def get_password_hash(password: str) -> str:
    """获取密码哈希"""
    return pwd_context.hash(password)

# 使用
hashed = get_password_hash("mysecretpassword")
print(hashed)  # $2b$12$...

is_valid = verify_password("mysecretpassword", hashed)
print(is_valid)  # True
```

---

## 2. JWT 认证

### 2.1 安装依赖

```bash
pip install python-jose[cryptography] PyJWT
```

### 2.2 JWT 工具

```python
# utils/jwt.py
from datetime import datetime, timedelta
from typing import Optional
from jose import JWTError, jwt
from passlib.context import CryptContext

# 配置
SECRET_KEY = "your-secret-key-change-in-production"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30
REFRESH_TOKEN_EXPIRE_DAYS = 7

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

# 创建访问令牌
def create_access_token(data: dict, expires_delta: Optional[timedelta] = None):
    to_encode = data.copy()
    
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

# 创建刷新令牌
def create_refresh_token(data: dict):
    to_encode = data.copy()
    expire = datetime.utcnow() + timedelta(days=REFRESH_TOKEN_EXPIRE_DAYS)
    to_encode.update({"exp": expire, "type": "refresh"})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

# 验证令牌
def verify_token(token: str) -> Optional[dict]:
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        return payload
    except JWTError:
        return None

# 获取当前用户
def get_current_user(token: str) -> Optional[str]:
    payload = verify_token(token)
    if payload is None:
        return None
    username: str = payload.get("sub")
    return username
```

---

## 3. OAuth2 密码模式

### 3.1 实现登录 API

```python
# auth.py
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel
from typing import Optional
from datetime import timedelta

app = FastAPI()

# OAuth2 配置
oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

# 模型
class Token(BaseModel):
    access_token: str
    refresh_token: str
    token_type: str

class TokenData(BaseModel):
    username: Optional[str] = None

class User(BaseModel):
    username: str
    email: Optional[str] = None
    full_name: Optional[str] = None

# 模拟用户数据
FAKE_USER_DB = {
    "johndoe": {
        "username": "johndoe",
        "full_name": "John Doe",
        "email": "johndoe@example.com",
        "hashed_password": "$2b$12$..."  # password: secretret
    }
}

# 验证用户
def verify_user(username: str, password: str):
    user = FAKE_USER_DB.get(username)
    if not user:
        return False
    if not pwd_context.verify(password, user["hashed_password"]):
        return False
    return user

# 登录接口
@app.post("/token", response_model=Token)
async def login(form_data: OAuth2PasswordRequestForm = Depends()):
    user = verify_user(form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="用户名或密码错误",
            headers={"WWW-Authenticate": "Bearer"},
        )
    
    # 创建令牌
    access_token = create_access_token(
        data={"sub": user["username"]},
        expires_delta=timedelta(minutes=30)
    )
    refresh_token = create_refresh_token(data={"sub": user["username"]})
    
    return {
        "access_token": access_token,
        "refresh_token": refresh_token,
        "token_type": "bearer"
    }

# 获取当前用户（受保护接口）
@app.get("/users/me", response_model=User)
async def read_users_me(token: str = Depends(oauth2_scheme)):
    payload = verify_token(token)
    if payload is None:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="无效的令牌",
        )
    
    username: str = payload.get("sub")
    user = FAKE_USER_DB.get(username)
    
    if user is None:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="用户不存在",
        )
    
    return User(**user)
```

### 3.2 使用 Bearer Token

```python
# 前端请求示例
import requests

# 登录
response = requests.post(
    "http://localhost:8000/token",
    data={
        "username": "johndoe",
        "password": "secretret"
    }
)
token_data = response.json()
access_token = token_data["access_token"]

# 使用 token 访问受保护接口
headers = {"Authorization": f"Bearer {access_token}"}
response = requests.get("http://localhost:8000/users/me", headers=headers)
print(response.json())
```

---

## 4. 刷新令牌

### 4.1 刷新接口

```python
# refresh_token.py
from fastapi import APIRouter, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer

router = APIRouter()

@app.post("/refresh", response_model=Token)
async def refresh_access_token(refresh_token: str = Depends(oauth2_scheme)):
    payload = verify_token(refresh_token)
    
    if payload is None:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="无效的刷新令牌",
        )
    
    if payload.get("type") != "refresh":
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="无效的令牌类型",
        )
    
    username = payload.get("sub")
    
    # 创建新令牌
    new_access_token = create_access_token(data={"sub": username})
    new_refresh_token = create_refresh_token(data={"sub": username})
    
    return {
        "access_token": new_access_token,
        "refresh_token": new_refresh_token,
        "token_type": "bearer"
    }
```

---

## 5. 依赖注入认证

### 5.1 创建认证依赖

```python
# dependencies.py
from fastapi import Depends, HTTPException, status
from fastapi.security import OAuth2PasswordBearer
from typing import Optional

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

async def get_current_user(token: str = Depends(oauth2_scheme)) -> str:
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="无法验证凭据",
        headers={"WWW-Authenticate": "Bearer"},
    )
    
    payload = verify_token(token)
    if payload is None:
        raise credentials_exception
    
    username: str = payload.get("sub")
    if username is None:
        raise credentials_exception
    
    return username

# 可选的认证（用户可能未登录）
async def get_current_user_optional(token: Optional[str] = Depends(oauth2_scheme)) -> Optional[str]:
    if token is None:
        return None
    
    try:
        return get_current_user(token)
    except HTTPException:
        return None
```

### 5.2 使用依赖

```python
# routes.py
from fastapi import APIRouter, Depends

router = APIRouter()

@router.get("/profile")
async def get_profile(current_user: str = Depends(get_current_user)):
    return {"username": current_user}

@router.get("/preferences")
async def get_preferences(current_user: Optional[str] = Depends(get_current_user_optional)):
    if current_user:
        return {"user": current_user, "theme": "dark"}
    return {"user": None, "theme": "light"}
```

---

## 6. 角色权限

### 6.1 定义角色

```python
# models.py
from enum import Enum
from pydantic import BaseModel

class UserRole(str, Enum):
    ADMIN = "admin"
    MODERATOR = "moderator"
    USER = "user"

class User(BaseModel):
    username: str
    role: UserRole = UserRole.USER

# 在 JWT 中添加角色
def create_access_token_with_role(data: dict, role: str):
    to_encode = data.copy()
    to_encode.update({"role": role, "exp": datetime.utcnow() + timedelta(minutes=30)})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
```

### 6.2 角色检查依赖

```python
# role_checker.py
from fastapi import Depends, HTTPException, status
from typing import List

class RoleChecker:
    def __init__(self, allowed_roles: List[str]):
        self.allowed_roles = allowed_roles
    
    def __call__(self, current_user: dict = Depends(get_current_user)):
        if current_user.get("role") not in self.allowed_roles:
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail="权限不足"
            )
        return current_user

# 使用
admin_only = RoleChecker(["admin"])
moderator_or_admin = RoleChecker(["admin", "moderator"])

@router.delete("/users/{user_id}")
async def delete_user(
    user_id: int,
    current_user: dict = Depends(admin_only)
):
    # 删除用户逻辑
    pass
```

---

## 7. API 安全防护

### 7.1 限流

```bash
pip install slowapi
```

```python
# rate_limit.py
from fastapi import FastAPI, Request, HTTPException
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)
app = FastAPI()

@app.post("/login")
@limiter.limit("5/minute")  # 每分钟5次
async def login(request: Request):
    # 登录逻辑
    pass

@app.get("/api")
@limiter.limit("100/minute")
async def api_endpoint(request: Request):
    return {"message": "OK"}
```

### 7.2 CORS 配置

```python
# cors.py
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://your-domain.com"],
    allow_credentials=True,
    allow_methods=["GET", "POST"],
    allow_headers=["Authorization", "Content-Type"],
)
```

### 7.3 SQL 注入防护

```python
# SQLAlchemy 自动防护
# 使用参数化查询，自动防止 SQL 注入
user = db.query(User).filter(User.username == username).first()

# 禁止使用原始 SQL
# ❌ db.execute(f"SELECT * FROM users WHERE username = '{username}'")
```

### 7.4 XSS 防护

```python
# 自动由 FastAPI 处理 JSON 响应转义
# 前端渲染时使用框架的自动转义

# Content Security Policy
@app.middleware("http")
async def add_csp_header(request, call_next):
    response = await call_next(request)
    response.headers["Content-Security-Policy"] = "default-src 'self'"
    return response
```

---

## 📝 课后练习

```python
# ========== 练习 1：密码哈希 ==========
# 1. 实现密码加密和验证函数
# 2. 在用户注册时加密密码

# ========== 练习 2：JWT 认证 ==========
# 1. 实现登录接口，返回 JWT
# 2. 实现受保护的接口

# ========== 练习 3：刷新令牌 ==========
# 1. 实现刷新令牌接口
# 2. 实现令牌过期处理

# ========== 练习 4：角色权限 ==========
# 1. 实现角色检查依赖
# 2. 实现管理员专用接口

# ========== 练习 5：安全防护 ==========
# 1. 实现登录限流
# 2. 配置 CORS
```

---

## 🎯 今日自检清单

- [ ] 密码哈希（bcrypt）
- [ ] JWT 认证实现
- [ ] OAuth2 密码模式
- [ ] 刷新令牌
- [ ] 角色权限控制
- [ ] API 安全防护

---

## 下一步

明天我们将学习：**Redis 缓存系统**
