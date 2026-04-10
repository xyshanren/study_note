> 学习日期：2026-04-29（周二）

## 学习目标

1. 掌握 SQLAlchemy 数据库集成
2. 理解异步支持
3. 掌握 WebSocket 实时通信
4. 学会使用中间件

---

## 1. 数据库集成

### 1.1 安装 SQLAlchemy

```bash
pip install sqlalchemy sqlalchemy[asyncio] asyncpg aiomysql
```

### 1.2 数据库配置

```python
# database.py
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from sqlalchemy.pool import StaticPool

# SQLite 示例（开发用）
SQLALCHEMY_DATABASE_URL = "sqlite:///./test.db"

engine = create_engine(
    SQLALCHEMY_DATABASE_URL,
    connect_args={"check_same_thread": False},
    poolclass=StaticPool,
)

# PostgreSQL 示例（生产用）
# SQLALCHEMY_DATABASE_URL = "postgresql+asyncpg://user:password@localhost/dbname"
# engine = create_async_engine(SQLALCHEMY_DATABASE_URL)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()

# 获取数据库会话
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### 1.3 定义模型

```python
# models.py
from sqlalchemy import Column, Integer, String, Boolean, DateTime, ForeignKey
from sqlalchemy.orm import relationship
from datetime import datetime
from database import Base

class User(Base):
    __tablename__ = "users"
    
    id = Column(Integer, primary_key=True, index=True)
    username = Column(String, unique=True, index=True, nullable=False)
    email = Column(String, unique=True, index=True, nullable=False)
    hashed_password = Column(String, nullable=False)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # 关联
    items = relationship("Item", back_populates="owner")

class Item(Base):
    __tablename__ = "items"
    
    id = Column(Integer, primary_key=True, index=True)
    title = Column(String, index=True, nullable=False)
    description = Column(String)
    owner_id = Column(Integer, ForeignKey("users.id"))
    
    owner = relationship("User", back_populates="items")

# 创建表
Base.metadata.create_all(bind=engine)
```

### 1.4 CRUD 操作

```python
# crud.py
from sqlalchemy.orm import Session
from models import User, Item
from pydantic import BaseModel
from typing import Optional

# Pydantic 模型
class UserCreate(BaseModel):
    username: str
    email: str
    password: str

class ItemCreate(BaseModel):
    title: str
    description: Optional[str] = None

# 创建用户
def create_user(db: Session, user: UserCreate):
    from passlib.context import CryptContext
    pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
    
    db_user = User(
        username=user.username,
        email=user.email,
        hashed_password=pwd_context.hash(user.password)
    )
    db.add(db_user)
    db.commit()
    db.refresh(db_user)
    return db_user

# 获取用户
def get_user(db: Session, user_id: int):
    return db.query(User).filter(User.id == user_id).first()

def get_user_by_email(db: Session, email: str):
    return db.query(User).filter(User.email == email).first()

# 获取用户列表
def get_users(db: Session, skip: int = 0, limit: int = 100):
    return db.query(User).offset(skip).limit(limit).all()

# 创建物品
def create_item(db: Session, item: ItemCreate, owner_id: int):
    db_item = Item(**item.dict(), owner_id=owner_id)
    db.add(db_item)
    db.commit()
    db.refresh(db_item)
    return db_item

# 获取物品列表
def get_items(db: Session, skip: int = 0, limit: int = 100):
    return db.query(Item).offset(skip).limit(limit).all()
```

### 1.5 API 集成

```python
# main.py
from fastapi import FastAPI, Depends, HTTPException, status
from sqlalchemy.orm import Session
from typing import List
from database import engine, get_db
from models import Base
from crud import (
    create_user, get_user, get_users,
    create_item, get_items
)
from schemas import UserCreate, UserResponse, ItemCreate, ItemResponse

app = FastAPI()

# 创建表
Base.metadata.create_all(bind=engine)

# 用户 API
@app.post("/users/", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
def create_new_user(user: UserCreate, db: Session = Depends(get_db)):
    db_user = get_user_by_email(db, email=user.email)
    if db_user:
        raise HTTPException(status_code=400, detail="邮箱已注册")
    return create_user(db=db, user=user)

@app.get("/users/", response_model=List[UserResponse])
def read_users(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    users = get_users(db, skip=skip, limit=limit)
    return users

@app.get("/users/{user_id}", response_model=UserResponse)
def read_user(user_id: int, db: Session = Depends(get_db)):
    user = get_user(db, user_id=user_id)
    if user is None:
        raise HTTPException(status_code=404, detail="用户不存在")
    return user

# 物品 API
@app.post("/users/{user_id}/items/", response_model=ItemResponse)
def create_item_for_user(
    user_id: int,
    item: ItemCreate,
    db: Session = Depends(get_db)
):
    return create_item(db=db, item=item, owner_id=user_id)

@app.get("/items/", response_model=List[ItemResponse])
def read_items(skip: int = 0, limit: int = 100, db: Session = Depends(get_db)):
    items = get_items(db, skip=skip, limit=limit)
    return items
```

---

## 2. 异步支持

### 2.1 异步 SQLAlchemy

```python
# async_database.py
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession, async_sessionmaker
from sqlalchemy.orm import declarative_base

SQLALCHEMY_DATABASE_URL = "sqlite+aiosqlite:///./test.db"

async_engine = create_async_engine(SQLALCHEMY_DATABASE_URL)
async_session = async_sessionmaker(async_engine, class_=AsyncSession)

Base = declarative_base()

async def get_db():
    async with async_session() as session:
        yield session
```

### 2.2 异步 CRUD

```python
# async_crud.py
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select
from models import User

async def create_user_async(db: AsyncSession, username: str, email: str):
    user = User(username=username, email=email)
    db.add(user)
    await db.commit()
    await db.refresh(user)
    return user

async def get_users_async(db: AsyncSession):
    result = await db.execute(select(User))
    return result.scalars().all()

async def get_user_by_id_async(db: AsyncSession, user_id: int):
    result = await db.execute(select(User).where(User.id == user_id))
    return result.scalar_one_or_none()
```

### 2.3 异步 API

```python
# async_main.py
from fastapi import FastAPI, Depends
from sqlalchemy.ext.asyncio import AsyncSession
from typing import List
from async_database import get_db, async_engine, Base
from async_crud import create_user_async, get_users_async
from pydantic import BaseModel

app = FastAPI()

# 创建表
async def init_db():
    async with async_engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)

@app.on_event("startup")
async def startup():
    await init_db()

# 模型
class UserCreate(BaseModel):
    username: str
    email: str

class UserResponse(BaseModel):
    id: int
    username: str
    email: str
    
    class Config:
        from_attributes = True

# 异步路由
@app.post("/users/", response_model=UserResponse)
async def create_user(
    user: UserCreate,
    db: AsyncSession = Depends(get_db)
):
    return await create_user_async(db, user.username, user.email)

@app.get("/users/", response_model=List[UserResponse])
async def read_users(db: AsyncSession = Depends(get_db)):
    return await get_users_async(db)
```

---

## 3. WebSocket 实时通信

### 3.1 基础 WebSocket

```python
# websocket.py
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from typing import List

app = FastAPI()

# 连接管理器
class ConnectionManager:
    def __init__(self):
        self.active_connections: List[WebSocket] = []
    
    async def connect(self, websocket: WebSocket):
        await websocket.accept()
        self.active_connections.append(websocket)
    
    def disconnect(self, websocket: WebSocket):
        self.active_connections.remove(websocket)
    
    async def send_personal_message(self, message: str, websocket: WebSocket):
        await websocket.send_text(message)
    
    async def broadcast(self, message: str):
        for connection in self.active_connections:
            await connection.send_text(message)

manager = ConnectionManager()

@app.websocket("/ws/{client_id}")
async def websocket_endpoint(websocket: WebSocket, client_id: str):
    await manager.connect(websocket)
    try:
        while True:
            data = await websocket.receive_text()
            # 处理消息
            await manager.send_personal_message(f"收到: {data}", websocket)
            # 广播给所有人
            await manager.broadcast(f"用户 {client_id}: {data}")
    except WebSocketDisconnect:
        manager.disconnect(websocket)
        await manager.broadcast(f"用户 {client_id} 离开")
```

### 3.2 WebSocket 房间

```python
# websocket_rooms.py
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from typing import Dict, List

app = FastAPI()

class RoomManager:
    def __init__(self):
        self.rooms: Dict[str, List[WebSocket]] = {}
    
    async def join_room(self, room_id: str, websocket: WebSocket):
        await websocket.accept()
        if room_id not in self.rooms:
            self.rooms[room_id] = []
        self.rooms[room_id].append(websocket)
    
    async def leave_room(self, room_id: str, websocket: WebSocket):
        if room_id in self.rooms:
            self.rooms[room_id].remove(websocket)
            if not self.rooms[room_id]:
                del self.rooms[room_id]
    
    async def broadcast_to_room(self, room_id: str, message: str):
        if room_id in self.rooms:
            for connection in self.rooms[room_id]:
                await connection.send_text(message)

room_manager = RoomManager()

@app.websocket("/ws/room/{room_id}")
async def websocket_room(websocket: WebSocket, room_id: str):
    await room_manager.join_room(room_id, websocket)
    try:
        while True:
            data = await websocket.receive_text()
            await room_manager.broadcast_to_room(room_id, data)
    except WebSocketDisconnect:
        await room_manager.leave_room(room_id, websocket)
```

---

## 4. 中间件

### 4.1 基础中间件

```python
# middleware.py
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware
from starlette.responses import Response
import time

# 方式1：使用 FastAPI 内置中间件
from starlette.middleware.cors import CORSMiddleware
from starlette.middleware.gzip import GZipMiddleware

app = FastAPI()

# CORS 中间件
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# Gzip 压缩
app.add_middleware(GZipMiddleware, minimum_size=1000)

# 方式2：自定义中间件
class TimingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        start_time = time.time()
        response = await call_next(request)
        process_time = time.time() - start_time
        response.headers["X-Process-Time"] = str(process_time)
        return response

app.add_middleware(TimingMiddleware)
```

### 4.2 请求日志中间件

```python
# logging_middleware.py
from fastapi import FastAPI, Request
from starlette.middleware.base import BaseHTTPMiddleware
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class LoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        logger.info(f"请求: {request.method} {request.url}")
        
        response = await call_next(request)
        
        logger.info(f"响应: {response.status_code}")
        return response

app = FastAPI()
app.add_middleware(LoggingMiddleware)
```

---

## 5. 静态文件与模板

### 5.1 静态文件

```python
# static_files.py
from fastapi import FastAPI
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates
from fastapi.requests import Request
from fastapi.responses import HTMLResponse

app = FastAPI()

# 静态文件
app.mount("/static", StaticFiles(directory="static"), name="static")

# 模板
templates = Jinja2Templates(directory="templates")

@app.get("/", response_class=HTMLResponse)
async def read_root(request: Request):
    return templates.TemplateResponse("index.html", {"request": request})

# templates/index.html
# <!DOCTYPE html>
# <html>
# <head><title>FastAPI</title></head>
# <body>
#     <h1>Hello, {{ name }}!</h1>
# </body>
# </html>
```

---

## 📝 课后练习

```python
# ========== 练习 1：数据库 CRUD ==========
# 1. 使用 SQLAlchemy 创建文章表
# 2. 实现文章的增删改查 API

# ========== 练习 2：异步数据库 ==========
# 1. 将上述操作改为异步版本
# 2. 使用 async/await

# ========== 练习 3：WebSocket ==========
# 1. 实现一个实时聊天功能
# 2. 支持多人聊天

# ========== 练习 4：中间件 ==========
# 1. 实现请求日志中间件
# 2. 实现认证中间件
```

---

## 🎯 今日自检清单

- [ ] SQLAlchemy 数据库集成
- [ ] CRUD 操作
- [ ] 异步支持
- [ ] WebSocket 实时通信
- [ ] 中间件使用
- [ ] 静态文件和模板

---

## 下一步

明天我们将学习：**FastAPI 认证与安全**
