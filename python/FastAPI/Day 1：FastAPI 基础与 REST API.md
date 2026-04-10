> 学习日期：2026-04-28（周一）

## 学习目标

1. 掌握 FastAPI 安装与基础用法
2. 理解路由、参数、请求体
3. 能够创建简单的 REST API

---

## 1. FastAPI 简介

FastAPI 是一个现代、快速的 Python Web 框架，基于 Python 类型提示。

### 1.1 安装

```bash
# 创建虚拟环境
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate  # Windows

# 安装 FastAPI 和 uvicorn
pip install fastapi
pip install "uvicorn[standard]"
```

### 1.2 第一个 API

```python
# main.py
from fastapi import FastAPI
from typing import Optional

app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI!"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return {
        "item_id": item_id,
        "query": q
    }

# 启动服务
# uvicorn main:app --reload
```

---

## 2. 路由与参数

### 2.1 路径参数

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int):
    """路径参数会自动进行类型转换"""
    return {"user_id": user_id, "name": f"用户{user_id}"}

@app.get("/files/{file_path:path}")
def read_file(file_path: str):
    """path 类型可以匹配路径"""
    return {"file_path": file_path}
```

### 2.2 查询参数

```python
from fastapi import FastAPI
from typing import Optional

app = FastAPI()

# 可选查询参数
@app.get("/items/")
def read_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}

# 必填查询参数
@app.get("/search/")
def search(q: str, limit: int = 10):
    return {"q": q, "limit": limit}

# 布尔类型参数
@app.get("/items/{item_id}")
def read_item(item_id: int, include_extra: bool = False):
    return {
        "item_id": item_id,
        "include_extra": include_extra
    }
```

### 2.3 请求体（POST）

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Optional

app = FastAPI()

# 定义数据模型
class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None

@app.post("/items/")
def create_item(item: Item):
    """请求体会自动进行 JSON -> Pydantic 模型转换"""
    return {"item": item, "message": "创建成功"}

# 使用模型（示例）
# {
#     "name": "iPhone",
#     "description": "智能手机",
#     "price": 6999.0,
#     "tax": 699.9
# }
```

---

## 3. Pydantic 数据验证

### 3.1 基础模型

```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

class User(BaseModel):
    id: int
    name: str
    email: str
    age: Optional[int] = None
    created_at: datetime = Field(default_factory=datetime.now)

# 使用
user = User(id=1, name="张三", email="zhangsan@example.com")
print(user)
```

### 3.2 数据验证

```python
from pydantic import BaseModel, Field, validator
from typing import List

class Product(BaseModel):
    name: str = Field(..., min_length=1, max_length=100)
    price: float = Field(..., gt=0)  # gt: greater than
    quantity: int = Field(default=0, ge=0)  # ge: greater than or equal
    tags: List[str] = Field(default_factory=list)
    
    @validator('name')
    def name_must_be_valid(cls, v):
        if 'xxx' in v.lower():
            raise ValueError('名称不能包含 xxx')
        return v.strip()

# 自动验证
try:
    product = Product(name="  ", price=-10)  # 会抛出验证错误
except Exception as e:
    print(e)
```

### 3.3 嵌套模型

```python
from pydantic import BaseModel
from typing import List, Optional

class Address(BaseModel):
    city: str
    street: str
    zip_code: str

class User(BaseModel):
    id: int
    name: str
    email: str
    address: Optional[Address] = None
    friends: List[int] = []

# 嵌套数据
user_data = {
    "id": 1,
    "name": "张三",
    "email": "zhangsan@example.com",
    "address": {
        "city": "北京",
        "street": "朝阳区",
        "zip_code": "100000"
    },
    "friends": [2, 3, 4]
}

user = User(**user_data)
```

---

## 4. 响应模型

### 4.1 定义响应模型

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr
from typing import Optional, List
from datetime import datetime

app = FastAPI()

# 请求模型
class UserCreate(BaseModel):
    name: str
    email: EmailStr
    password: str

# 响应模型（不包含敏感信息）
class UserResponse(BaseModel):
    id: int
    name: str
    email: str
    created_at: datetime
    
    class Config:
        from_attributes = True

# 使用 response_model
@app.post("/users/", response_model=UserResponse)
def create_user(user: UserCreate):
    # 模拟创建用户
    return {
        "id": 1,
        "name": user.name,
        "email": user.email,
        "created_at": datetime.now()
    }
```

### 4.2 列表响应

```python
from typing import List

@app.get("/users/", response_model=List[UserResponse])
def get_users():
    return [
        {"id": 1, "name": "张三", "email": "zhangsan@example.com", "created_at": datetime.now()},
        {"id": 2, "name": "李四", "email": "lisi@example.com", "created_at": datetime.now()}
    ]
```

---

## 5. 错误处理

### 5.1 HTTP 异常

```python
from fastapi import HTTPException, status

@app.get("/items/{item_id}")
def read_item(item_id: int):
    if item_id == 0:
        raise HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail="ID 不能为 0"
        )
    return {"item_id": item_id}
```

### 5.2 自定义异常处理器

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse
from fastapi.exceptions import RequestValidationError

app = FastAPI()

# 自定义异常
class CustomException(Exception):
    def __init__(self, message: str):
        self.message = message

@app.exception_handler(CustomException)
async def custom_exception_handler(request: Request, exc: CustomException):
    return JSONResponse(
        status_code=418,
        content={"message": exc.message}
    )

@app.get("/custom/")
def custom_endpoint():
    raise CustomException("这是一个自定义错误")
```

### 5.3 验证错误处理

```python
@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=422,
        content={
            "detail": exc.errors(),
            "body": exc.body
        }
    )
```

---

## 6. 依赖注入

### 6.1 基础依赖

```python
from fastapi import FastAPI, Depends
from typing import Optional

app = FastAPI()

# 定义依赖函数
def get_query_param(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}

# 使用依赖
@app.get("/items/")
def read_items(params: dict = Depends(get_query_param)):
    return params

# 类作为依赖
class QueryParams:
    def __init__(self, skip: int = 0, limit: int = 10):
        self.skip = skip
        self.limit = limit

@app.get("/items2/")
def read_items2(params: QueryParams = Depends()):
    return {"skip": params.skip, "limit": params.limit}
```

### 6.2 认证依赖

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBasic, HTTPBasicCredentials

security = HTTPBasic()

def get_current_user(credentials: HTTPBasicCredentials = Depends(security)):
    if credentials.username != "admin" or credentials.password != "secret":
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="认证失败",
            headers={"WWW-Authenticate": "Basic"},
        )
    return {"username": credentials.username}

@app.get("/protected/")
def protected_route(user: dict = Depends(get_current_user)):
    return user
```

---

## 📝 课后练习

```python
# ========== 练习 1：基础 API ==========
# 1. 创建一个用户管理 API
# 2. 实现用户列表、创建用户接口

# ========== 练习 2：数据验证 ==========
# 1. 使用 Pydantic 创建用户注册模型
# 2. 添加邮箱、密码验证

# ========== 练习 3：响应模型 ==========
# 1. 创建请求和响应模型
# 2. 确保响应不包含密码

# ========== 练习 4：依赖注入 ==========
# 1. 实现分页依赖
# 2. 实现认证依赖
```

---

## 🎯 今日自检清单

- [ ] FastAPI 安装与基础用法
- [ ] 路由与参数（路径、查询、请求体）
- [ ] Pydantic 数据验证
- [ ] 响应模型
- [ ] 错误处理
- [ ] 依赖注入

---

## 下一步

明天我们将学习：**FastAPI 高级特性（数据库、WebSocket、中间件）**
