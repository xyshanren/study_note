> 学习日期：2026-05-02（周五）

## 学习目标

1. 掌握 MongoDB 基础操作
2. 理解文档模型设计
3. 能够在 FastAPI 中集成 MongoDB

---

## 1. MongoDB 基础

### 1.1 安装

```bash
# Ubuntu
sudo apt-get install mongodb

# macOS
brew tap mongodb/brew
brew install mongodb-community

# Docker
docker run -d -p 27017:27017 mongo
```

### 1.2 Python MongoDB 驱动

```bash
pip install motor
```

---

## 2. MongoDB 基础操作

### 2.1 连接与基础操作

```python
# basic_mongo.py
import motor.motor_asyncio
import asyncio

# 异步连接
client = motor.motor_asyncio.AsyncIOClientMotorClient('mongodb://localhost:27017')
db = client['my_database']

# 集合
collection = db['users']

async def basic_operations():
    # 插入文档
    user = {
        "name": "张三",
        "email": "zhangsan@example.com",
        "age": 25,
        "tags": ["python", "fastapi"]
    }
    result = await collection.insert_one(user)
    print(f"插入ID: {result.inserted_id}")
    
    # 查询单个文档
    user = await collection.find_one({"name": "张三"})
    print(user)
    
    # 查询多个文档
    async for user in collection.find({"age": {"$gte": 20}}):
        print(user)
    
    # 更新文档
    await collection.update_one(
        {"name": "张三"},
        {"$set": {"age": 26}}
    )
    
    # 删除文档
    await collection.delete_one({"name": "张三"})

asyncio.run(basic_operations())
```

### 2.2 查询操作

```python
# queries.py

# 基础查询
await collection.find_one({"name": "张三"})
await collection.find({"age": 25})

# 比较运算符
{"age": {"$gt": 20}}           # 大于
{"age": {"$gte": 20}}          # 大于等于
{"age": {"$lt": 30}}           # 小于
{"age": {"$lte": 30}}          # 小于等于
{"age": {"$ne": 25}}           # 不等于
{"age": {"$in": [20, 25, 30]}} # 在数组中
{"age": {"$nin": [20, 25]}}   # 不在数组中

# 逻辑运算符
{"$and": [{"age": {"$gt": 20}}, {"age": {"$lt": 30}}]}
{"$or": [{"name": "张三"}, {"name": "李四"}]}
{"$not": {"age": {"$gt": 20}}}

# 字符串查询
{"name": {"$regex": "^张"}}              # 正则
{"name": {"$options": "i"}}              # 不区分大小写

# 数组查询
{"tags": "python"}                       # 包含元素
{"tags": {"$all": ["python", "fastapi"]}} # 包含所有元素
{"tags": {"$size": 2}}                   # 数组长度

# 分页
await collection.find().skip(10).limit(10)

# 排序
await collection.find().sort("age", 1)   # 升序
await collection.find().sort("age", -1)  # 降序
```

### 2.3 聚合管道

```python
# aggregation.py

# 基础聚合
pipeline = [
    {"$match": {"age": {"$gte": 20}}},   # 过滤
    {"$sort": {"age": 1}},               # 排序
    {"$limit": 10},                       # 限制
    {"$skip": 0}                          # 跳过
]

async for doc in collection.aggregate(pipeline):
    print(doc)

# 分组统计
pipeline = [
    {"$group": {
        "_id": "$age",
        "count": {"$sum": 1},
        "names": {"$push": "$name"}
    }}
]

# 计算平均值
pipeline = [
    {"$group": {
        "_id": None,
        "avg_age": {"$avg": "$age"},
        "total": {"$sum": 1}
    }}
]

# 多表聚合
pipeline = [
    {"$lookup": {
        "from": "orders",
        "localField": "_id",
        "foreignField": "user_id",
        "as": "orders"
    }}
]
```

---

## 3. Pydantic 模型

### 3.1 MongoDB 模型

```python
# models.py
from pydantic import BaseModel, Field, EmailStr
from typing import Optional, List
from datetime import datetime
from enum import Enum

class UserRole(str, Enum):
    ADMIN = "admin"
    USER = "user"

class UserBase(BaseModel):
    username: str = Field(..., min_length=3, max_length=50)
    email: EmailStr

class UserCreate(UserBase):
    password: str = Field(..., min_length=6)

class UserUpdate(BaseModel):
    username: Optional[str] = None
    email: Optional[EmailStr] = None
    bio: Optional[str] = None

class UserResponse(UserBase):
    id: str = Field(..., alias="_id")
    role: UserRole = Role.USER
    created_at: datetime
    is_active: bool = True
    
    class Config:
        populate_by_name = True
        from_attributes = True

# 文章模型
class Article(BaseModel):
    title: str = Field(..., min_length=1, max_length=200)
    content: str
    author_id: str
    tags: List[str] = []
    published: bool = False
    created_at: datetime = Field(default_factory=datetime.utcnow)
    updated_at: datetime = Field(default_factory=datetime.utcnow)
```

---

## 4. CRUD 操作

### 4.1 Repository 模式

```python
# repository.py
from motor.motor_asyncio import AsyncIOMotorClient, AsyncIOMotorDatabase
from typing import Optional, List, TypeVar, Generic
from pydantic import BaseModel
from datetime import datetime

T = TypeVar('T', bound=BaseModel)

class MongoRepository(Generic[T]):
    def __init__(self, collection: AsyncIOMotorClient, model_class: type[T]):
        self.collection = collection
        self.model_class = model_class
    
    async def create(self, data: dict) -> T:
        data['created_at'] = datetime.utcnow()
        data['updated_at'] = datetime.utcnow()
        result = await self.collection.insert_one(data)
        data['_id'] = result.inserted_id
        return self.model_class(**data)
    
    async def get(self, id: str) -> Optional[T]:
        from bson import ObjectId
        doc = await self.collection.find_one({"_id": ObjectId(id)})
        if doc:
            return self.model_class(**doc)
        return None
    
    async def find_one(self, query: dict) -> Optional[T]:
        doc = await self.collection.find_one(query)
        if doc:
            return self.model_class(**doc)
        return None
    
    async def find_many(
        self, 
        query: dict = {}, 
        skip: int = 0, 
        limit: int = 100,
        sort: tuple = None
    ) -> List[T]:
        cursor = self.collection.find(query).skip(skip).limit(limit)
        if sort:
            cursor = cursor.sort(sort)
        
        results = []
        async for doc in cursor:
            results.append(self.model_class(**doc))
        return results
    
    async def update(self, id: str, data: dict) -> Optional[T]:
        from bson import ObjectId
        data['updated_at'] = datetime.utcnow()
        
        await self.collection.update_one(
            {"_id": ObjectId(id)},
            {"$set": data}
        )
        return await self.get(id)
    
    async def delete(self, id: str) -> bool:
        from bson import ObjectId
        result = await self.collection.delete_one({"_id": ObjectId(id)})
        return result.deleted_count > 0
    
    async def count(self, query: dict = {}) -> int:
        return await self.collection.count_documents(query)
```

### 4.2 用户 Repository

```python
# user_repository.py

class UserRepository(MongoRepository[UserResponse]):
    def __init__(self, db: AsyncIOMotorDatabase):
        super().__init__(db['users'], UserResponse)
    
    async def find_by_email(self, email: str) -> Optional[UserResponse]:
        return await self.find_one({"email": email})
    
    async def find_by_username(self, username: str) -> Optional[UserResponse]:
        return await self.find_one({"username": username})
    
    async def create_user(self, user_data: UserCreate) -> UserResponse:
        from passlib.context import CryptContext
        pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
        
        data = user_data.model_dump()
        data['hashed_password'] = pwd_context.hash(data.pop('password'))
        data['role'] = 'user'
        data['is_active'] = True
        
        return await self.create(data)
```

---

## 5. FastAPI 集成

### 5.1 依赖注入

```python
# deps.py
from fastapi import Depends
from motor.motor_asyncio import AsyncIOMotorClient, AsyncIOMotorDatabase
from typing import AsyncGenerator

MONGO_URL = "mongodb://localhost:27017"

async def get_database() -> AsyncGenerator[AsyncIOMotorDatabase, None]:
    client = AsyncIOMotorClient(MONGO_URL)
    try:
        yield client['my_database']
    finally:
        client.close()

# 使用
@app.get("/users/{user_id}")
async def get_user(
    user_id: str,
    db: AsyncIOMotorDatabase = Depends(get_database)
):
    from bson import ObjectId
    user = await db['users'].find_one({"_id": ObjectId(user_id)})
    return user
```

### 5.2 完整 API 示例

```python
# main.py
from fastapi import FastAPI, Depends, HTTPException, status
from motor.motor_asyncio import AsyncIOMotorDatabase
from typing import List
from bson import ObjectId

app = FastAPI()

# 依赖
async def get_db():
    from deps import get_database
    async for db in get_database():
        yield db

# 用户 API
@app.post("/users/", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
async def create_user(
    user: UserCreate,
    db: AsyncIOMotorDatabase = Depends(get_db)
):
    # 检查邮箱是否已存在
    existing = await db['users'].find_one({"email": user.email})
    if existing:
        raise HTTPException(status_code=400, detail="邮箱已注册")
    
    # 创建用户
    from passlib.context import CryptContext
    pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")
    
    user_data = user.model_dump()
    user_data['hashed_password'] = pwd_context.hash(user_data.pop('password'))
    
    result = await db['users'].insert_one(user_data)
    user_data['_id'] = result.inserted_id
    
    return user_data

@app.get("/users/", response_model=List[UserResponse])
async def list_users(
    skip: int = 0,
    limit: int = 10,
    db: AsyncIOMotorDatabase = Depends(get_db)
):
    users = await db['users'].find().skip(skip).limit(limit).to_list(length=limit)
    return users

@app.get("/users/{user_id}", response_model=UserResponse)
async def get_user(
    user_id: str,
    db: AsyncIOMotorDatabase = Depends(get_db)
):
    user = await db['users'].find_one({"_id": ObjectId(user_id)})
    if not user:
        raise HTTPException(status_code=404, detail="用户不存在")
    return user

@app.delete("/users/{user_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_user(
    user_id: str,
    db: AsyncIOMotorDatabase = Depends(get_db)
):
    result = await db['users'].delete_one({"_id": ObjectId(user_id)})
    if result.deleted_count == 0:
        raise HTTPException(status_code=404, detail="用户不存在")
```

---

## 6. 文档模型设计

### 6.1 嵌入 vs 引用

```python
# 嵌入文档：适用于 1:1 或 1:few 关系
{
    "name": "张三",
    "address": {
        "city": "北京",
        "street": "朝阳区"
    }
}

# 引用文档：适用于 1:many 或 many:many 关系
# 用户集合
{
    "_id": ObjectId("..."),
    "name": "张三"
}

# 订单集合
{
    "_id": ObjectId("..."),
    "user_id": ObjectId("用户ID"),
    "items": [...]
}
```

### 6.2 设计示例

```python
# 博客系统文档设计

# 文章文档（嵌入评论）
{
    "title": "FastAPI 教程",
    "content": "...",
    "author": {
        "id": ObjectId("..."),
        "name": "张三"
    },
    "tags": ["Python", "FastAPI"],
    "comments": [
        {
            "user": "李四",
            "content": "写得真好",
            "created_at": datetime
        }
    ],
    "stats": {
        "views": 100,
        "likes": 50
    }
}

# 用户文档（引用文章）
{
    "_id": ObjectId("..."),
    "username": "zhangsan",
    "email": "...",
    "articles": [ObjectId("..."), ObjectId("...")],
    "followers": [ObjectId("...")],
    "following": [ObjectId("...")]
}
```

---

## 📝 课后练习

```python
# ========== 练习 1：基础操作 ==========
# 1. 连接 MongoDB 并进行 CRUD 操作
# 2. 使用聚合管道进行统计

# ========== 练习 2：Pydantic 模型 ==========
# 1. 定义用户、文章模型
# 2. 实现数据验证

# ========== 练习 3：FastAPI 集成 ==========
# 1. 实现用户的 CRUD API
# 2. 实现分页查询

# ========== 练习 4：文档设计 ==========
# 1. 设计一个电商系统的文档结构
# 2. 决定使用嵌入还是引用
```

---

## 🎯 今日自检清单

- [ ] MongoDB 基础操作
- [ ] 查询与聚合管道
- [ ] Pydantic 模型
- [ ] Repository 模式
- [ ] FastAPI 集成
- [ ] 文档模型设计

---

## FastAPI 阶段总结

这周我们学习了 FastAPI：
1. ✅ Day 1: FastAPI 基础与 REST API
2. ✅ Day 2: 数据库集成、WebSocket、中间件
3. ✅ Day 3: 认证与安全
4. ✅ Day 4: Redis 缓存系统

---

## 下周预告

- Day 5: 鸿蒙 ArkTS 移动端开发
