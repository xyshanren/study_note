> 学习日期：2026-05-01（周四）

## 学习目标

1. 掌握 Redis 安装与基础操作
2. 理解缓存策略
3. 能够在 FastAPI 中集成 Redis

---

## 1. Redis 基础

### 1.1 安装 Redis

```bash
# Ubuntu/Debian
sudo apt-get install redis-server

# macOS
brew install redis

# 启动 Redis
redis-server
redis-cli  # 客户端连接
```

### 1.2 Python Redis 客户端

```bash
pip install redis
```

### 1.3 基础操作

```python
# basic_redis.py
import redis

# 连接 Redis
r = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

# 字符串操作
r.set('name', '张三')
print(r.get('name'))  # 张三

# 设置过期时间（秒）
r.setex('code', 300, '123456')  # 5分钟后过期

# 数字操作
r.set('counter', 0)
r.incr('counter')  # +1
r.decr('counter')  # -1

# 哈希操作
r.hset('user:1', 'name', '张三')
r.hset('user:1', 'age', '25')
print(r.hgetall('user:1'))  # {'name': '张三', 'age': '25'}

# 列表操作
r.lpush('tasks', 'task1')
r.lpush('tasks', 'task2')
print(r.lrange('tasks', 0, -1))  # ['task2', 'task1']

# 集合操作
r.sadd('tags', 'python', 'redis', 'fastapi')
print(r.smembers('tags'))  # {'python', 'redis', 'fastapi'}

# 删除
r.delete('name')
```

---

## 2. Redis 连接池

### 2.1 连接池配置

```python
# redis_pool.py
import redis
from redis import ConnectionPool

# 方式1：基础连接池
pool = ConnectionPool(
    host='localhost',
    port=6379,
    db=0,
    max_connections=10,
    decode_responses=True
)

r = redis.Redis(connection_pool=pool)

# 方式2：使用缓存装饰器
from functools import wraps

_pool = None

def get_redis():
    global _pool
    if _pool is None:
        _pool = ConnectionPool(host='localhost', port=6379, db=0)
    return redis.Redis(connection_pool=_pool)
```

### 2.2 异步 Redis

```bash
pip install aioredis
```

```python
# async_redis.py
import asyncio
import aioredis

async def main():
    # 连接
    redis = await aioredis.create_redis_pool('redis://localhost')
    
    # 设置
    await redis.set('key', 'value')
    
    # 获取
    value = await redis.get('key')
    print(value)
    
    # 关闭
    redis.close()
    await redis.wait_closed()

asyncio.run(main())
```

---

## 3. 缓存策略

### 3.1 缓存模式

```python
# cache_strategies.py
from typing import Optional, Callable, Any
import json
import redis
from functools import wraps

r = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

# 模式1：Cache-Aside（旁路缓存）
def cache_aside(key_prefix: str, expire: int = 300):
    """查询缓存，缓存不存在则查询数据库并写入缓存"""
    def decorator(func: Callable):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # 构建缓存 key
            cache_key = f"{key_prefix}:{args}:{kwargs}"
            
            # 1. 查询缓存
            cached = r.get(cache_key)
            if cached:
                return json.loads(cached)
            
            # 2. 查询数据库
            result = func(*args, **kwargs)
            
            # 3. 写入缓存
            r.setex(cache_key, expire, json.dumps(result))
            
            return result
        return wrapper
    return decorator

# 模式2：Write-Through（写穿透）
def write_through(key_prefix: str):
    """写入时同时更新缓存和数据库"""
    def decorator(func: Callable):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # 1. 写入数据库
            result = func(*args, **kwargs)
            
            # 2. 写入缓存
            cache_key = f"{key_prefix}:{args}"
            r.set(cache_key, json.dumps(result))
            
            return result
        return wrapper
    return decorator
```

### 3.2 缓存问题处理

```python
# cache_problems.py

# 1. 缓存穿透（请求不存在的数据）
def cache_aside_with_check(key_prefix: str, expire: int = 300):
    """添加空值缓存，防止缓存穿透"""
    def decorator(func: Callable):
        @wraps(func)
        def wrapper(*args, **kwargs):
            cache_key = f"{key_prefix}:{args}"
            
            cached = r.get(cache_key)
            if cached:
                if cached == "NULL":
                    return None
                return json.loads(cached)
            
            result = func(*args, **kwargs)
            
            # 缓存空值，设置较短过期时间
            if result is None:
                r.setex(cache_key, 60, "NULL")  # 缓存空值1分钟
            else:
                r.setex(cache_key, expire, json.dumps(result))
            
            return result
        return wrapper
    return decorator

# 2. 缓存雪崩（大量缓存同时过期）
def cache_with_random_expire(key_prefix: str, base_expire: int = 300):
    """添加随机过期时间"""
    import random
    expire = base_expire + random.randint(0, 60)
    
    def decorator(func: Callable):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # ... 缓存逻辑
            r.setex(cache_key, expire, json.dumps(result))
            return result
        return wrapper
    return decorator

# 3. 缓存击穿（热点 key 过期后大量请求）
def cache_with_lock(key_prefix: str, expire: int = 300):
    """使用分布式锁"""
    import time
    
    def decorator(func: Callable):
        @wraps(func)
        def wrapper(*args, **kwargs):
            cache_key = f"{key_prefix}:{args}"
            
            cached = r.get(cache_key)
            if cached:
                return json.loads(cached)
            
            # 获取锁
            lock_key = f"lock:{cache_key}"
            lock = r.set(lock_key, "1", nx=True, ex=10)
            
            if lock:
                # 查询数据库
                result = func(*args, **kwargs)
                r.setex(cache_key, expire, json.dumps(result))
                r.delete(lock_key)
            else:
                # 等待并重试获取缓存
                time.sleep(0.1)
                return wrapper(*args, **kwargs)
            
            return result
        return wrapper
    return decorator
```

---

## 4. FastAPI 集成 Redis

### 4.1 依赖注入

```python
# deps.py
from fastapi import Depends
import redis
from typing import Optional

_redis_pool = None

def get_redis() -> redis.Redis:
    global _redis_pool
    if _redis_pool is None:
        _redis_pool = redis.ConnectionPool(
            host='localhost',
            port=6379,
            db=0,
            decode_responses=True
        )
    return redis.Redis(connection_pool=_redis_pool)

# 使用
@app.get("/items/{item_id}")
async def get_item(item_id: int, r: redis.Redis = Depends(get_redis)):
    cached = r.get(f"item:{item_id}")
    if cached:
        return json.loads(cached)
    
    # 查询数据库...
    return {"item_id": item_id, "name": "Item"}
```

### 4.2 会话管理

```python
# session.py
from fastapi import FastAPI, Depends, HTTPException
from pydantic import BaseModel
import redis
import json
from datetime import timedelta
import uuid

app = FastAPI()

class SessionData(BaseModel):
    user_id: int
    username: str

def get_session_store():
    return redis.Redis(host='localhost', port=6379, db=1, decode_responses=True)

def create_session(r: redis.Redis, user_id: int, username: str) -> str:
    """创建会话"""
    session_id = str(uuid.uuid4())
    session_key = f"session:{session_id}"
    
    session_data = {
        "user_id": user_id,
        "username": username
    }
    
    r.setex(session_key, timedelta(days=7), json.dumps(session_data))
    return session_id

def get_session(r: redis.Redis, session_id: str) -> Optional[SessionData]:
    """获取会话"""
    session_key = f"session:{session_id}"
    data = r.get(session_key)
    
    if data:
        return SessionData(**json.loads(data))
    return None

def delete_session(r: redis.Redis, session_id: str):
    """删除会话"""
    r.delete(f"session:{session_id}")

# 登录
@app.post("/login")
async def login(
    username: str,
    password: str,
    r: redis.Redis = Depends(get_session_store)
):
    # 验证用户...
    session_id = create_session(r, user_id=1, username=username)
    
    return {"session_id": session_id}

# 获取用户信息
@app.get("/me")
async def get_me(
    session_id: str,
    r: redis.Redis = Depends(get_session_store)
):
    session = get_session(r, session_id)
    if not session:
        raise HTTPException(status_code=401, detail="未登录")
    
    return session

# 登出
@app.post("/logout")
async def logout(
    session_id: str,
    r: redis.Redis = Depends(get_session_store)
):
    delete_session(r, session_id)
    return {"message": "已登出"}
```

### 4.3 接口缓存

```python
# api_cache.py
from fastapi import FastAPI, Depends
from functools import wraps
import json
import redis

app = FastAPI()
r = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

def cached_route(expire: int = 300):
    """API 缓存装饰器"""
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            # 构建缓存 key
            cache_key = f"api:{func.__name__}:{args}:{kwargs}"
            
            # 查询缓存
            cached = r.get(cache_key)
            if cached:
                return json.loads(cached)
            
            # 执行原函数
            result = await func(*args, **kwargs)
            
            # 写入缓存
            r.setex(cache_key, expire, json.dumps(result))
            
            return result
        return wrapper
    return decorator

@app.get("/users/{user_id}")
@cached_route(expire=60)
async def get_user(user_id: int):
    # 模拟数据库查询
    return {"id": user_id, "name": f"用户{user_id}"}
```

---

## 5. 分布式锁

### 5.1 基础锁

```python
# distributed_lock.py
import redis
import time
import uuid

class DistributedLock:
    def __init__(self, redis_client: redis.Redis, key: str, expire: int = 10):
        self.redis = redis_client
        self.key = f"lock:{key}"
        self.expire = expire
        self.lock_value = str(uuid.uuid4())
    
    def acquire(self, timeout: int = 10) -> bool:
        """获取锁"""
        start_time = time.time()
        
        while time.time() - start_time < timeout:
            if self.redis.set(self.key, self.lock_value, nx=True, ex=self.expire):
                return True
            time.sleep(0.01)
        
        return False
    
    def release(self):
        """释放锁"""
        if self.redis.get(self.key) == self.lock_value:
            self.redis.delete(self.key)
    
    def __enter__(self):
        if not self.acquire():
            raise TimeoutError("获取锁失败")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.release()

# 使用
r = redis.Redis()
lock = DistributedLock(r, "payment:order:123")

try:
    with lock:
        # 处理支付逻辑
        pass
except TimeoutError:
    print("处理中，请稍后重试")
```

---

## 6. 消息队列

### 6.1 任务队列

```python
# task_queue.py
import redis
import json
from typing import Callable, Any

r = redis.Redis(host='localhost', port=6379, db=0)

# 生产者：添加任务
def add_task(queue_name: str, task_data: dict):
    r.lpush(queue_name, json.dumps(task_data))

# 消费者：处理任务
def process_task(queue_name: str, handler: Callable[[dict], Any]):
    while True:
        # 从队列获取任务（阻塞）
        result = r.brpop(queue_name, timeout=0)
        if result:
            _, task_json = result
            task = json.loads(task_json)
            handler(task)

# 示例
def send_email_handler(task: dict):
    print(f"发送邮件: {task['to']}")

# 添加任务
add_task("email_queue", {"to": "user@example.com", "subject": "Hello"})

# 处理任务（需要在独立进程中运行）
# process_task("email_queue", send_email_handler)
```

---

## 📝 课后练习

```python
# ========== 练习 1：Redis 基础 ==========
# 1. 连接 Redis 并进行基本操作
# 2. 实现用户会话存储

# ========== 练习 2：缓存策略 ==========
# 1. 实现 Cache-Aside 缓存模式
# 2. 处理缓存穿透问题

# ========== 练习 3：FastAPI 集成 ==========
# 1. 使用 Redis 缓存 API 响应
# 2. 实现登录会话管理

# ========== 练习 4：分布式锁 ==========
# 1. 实现一个简单的分布式锁
# 2. 使用锁保护并发操作
```

---

## 🎯 今日自检清单

- [ ] Redis 基础操作
- [ ] 连接池配置
- [ ] 缓存策略（Cache-Aside、Write-Through）
- [ ] 缓存问题（穿透、雪崩、击穿）
- [ ] FastAPI + Redis 集成
- [ ] 分布式锁

---

## 下一步

明天我们将学习：**MongoDB NoSQL 数据库**
