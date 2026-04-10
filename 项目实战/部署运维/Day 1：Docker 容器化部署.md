> 学习日期：2026-05-19（周一）

## 学习目标

1. 掌握 Docker 基础概念
2. 能够编写 Dockerfile
3. 理解 Docker Compose 多容器部署

---

## 1. Docker 基础

### 1.1 什么是 Docker

Docker 是一个开源的容器化平台，可以将应用及其依赖打包成轻量级的容器。

### 1.2 核心概念

| 概念 | 说明 |
|------|------|
| 镜像（Image） | 模板，只读 |
| 容器（Container） | 镜像的运行实例 |
| 仓库（Registry） | 存储镜像的地方 |

### 1.3 常用命令

```bash
# 镜像操作
docker pull nginx              # 拉取镜像
docker images                 # 列出镜像
docker rmi nginx              # 删除镜像
docker build -t myapp .       # 构建镜像

# 容器操作
docker run -d -p 8080:80 nginx    # 运行容器
docker ps                     # 列出运行中的容器
docker ps -a                 # 列出所有容器
docker stop container_id     # 停止容器
docker rm container_id        # 删除容器
docker logs -f container_id  # 查看日志
docker exec -it container_id bash  # 进入容器
```

---

## 2. Dockerfile 编写

### 2.1 基础 Dockerfile

```dockerfile
# 基础镜像
FROM node:18-alpine

# 设置工作目录
WORKDIR /app

# 复制依赖文件
COPY package*.json ./

# 安装依赖
RUN npm install

# 复制代码
COPY . .

# 暴露端口
EXPOSE 3000

# 启动命令
CMD ["npm", "start"]
```

### 2.2 多阶段构建

```dockerfile
# 构建阶段
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# 运行阶段
FROM node:18-alpine

WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules

EXPOSE 3000
CMD ["node", "dist/index.js"]
```

### 2.3 Python Dockerfile

```dockerfile
FROM python:3.11-slim

# 设置工作目录
WORKDIR /app

# 复制依赖文件
COPY requirements.txt .

# 安装依赖
RUN pip install --no-cache-dir -r requirements.txt

# 复制代码
COPY . .

# 暴露端口
EXPOSE 8000

# 启动命令
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 2.4 .dockerignore

```
node_modules
__pycache__
*.pyc
*.pyo
*.pyd
.git
.gitignore
README.md
.env
.env.local
dist
build
*.log
```

---

## 3. Docker Compose

### 3.1 docker-compose.yml

```yaml
version: '3.8'

services:
  # 前端
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
    depends_on:
      - backend

  # 后端
  backend:
    build: ./backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/ecommerce
      - REDIS_URL=redis://redis:6379
    depends_on:
      - db
      - redis

  # 数据库
  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=ecommerce
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  # Redis
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

  # Nginx 反向代理
  nginx:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - frontend
      - backend

volumes:
  postgres_data:
  redis_data:
```

### 2.3 常用命令

```bash
# 启动所有服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down

# 重建服务
docker-compose up -d --build

# 查看服务状态
docker-compose ps
```

---

## 4. 电商系统 Docker 配置

### 4.1 后端 Dockerfile

```dockerfile
# backend/Dockerfile
FROM python:3.11-slim

WORKDIR /app

# 安装系统依赖
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# 复制依赖文件
COPY requirements.txt .

# 安装 Python 依赖
RUN pip install --no-cache-dir -r requirements.txt

# 复制代码
COPY . .

# 创建数据库
RUN python -c "import sys; print('Waiting for DB...')"

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```

### 4.2 前端 Dockerfile

```dockerfile
# frontend/Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Nginx
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### 4.3 Nginx 配置

```nginx
# nginx.conf
server {
    listen 80;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api {
        proxy_pass http://backend:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    location /ws {
        proxy_pass http://backend:8000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

### 4.4 完整的 docker-compose.yml

```yaml
version: '3.8'

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:80"
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/ecommerce
      - REDIS_URL=redis://redis:6379
      - SECRET_KEY=your-secret-key
    depends_on:
      - db
      - redis
    volumes:
      - ./backend:/app

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=ecommerce
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

---

## 5. 优化与最佳实践

### 5.1 镜像优化

```dockerfile
# 使用 Alpine 镜像减小体积
FROM node:18-alpine

# 减少层数
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    package1 \
    package2 && \
    rm -rf /var/lib/apt/lists/*

# 使用 .dockerignore
# 排除不需要的文件

# 多阶段构建
# 分离构建和运行环境
```

### 5.2 安全建议

```dockerfile
# 不要使用 root 用户
FROM node:18-alpine
RUN addgroup -g 1001 appgroup && \
    adduser -u 1001 -G appgroup -s /bin/sh -D appuser

USER appuser

# 最小化权限
COPY --chown=appuser:appgroup . .
```

### 5.3 环境变量

```yaml
# 使用 .env 文件
# .env
# POSTGRES_PASSWORD=secretpassword
# SECRET_KEY=your-secret-key

# docker-compose.yml
services:
  backend:
    env_file:
      - .env
```

---

## 📝 课后练习

```bash
# ========== 练习 1：Docker 基础 ==========
# 1. 安装 Docker
# 2. 运行第一个容器

# ========== 练习 2：Dockerfile ==========
# 1. 为项目编写 Dockerfile
# 2. 构建并测试镜像

# ========== 练习 3：Docker Compose ==========
# 1. 编写 docker-compose.yml
# 2. 启动完整服务
```

---

## 🎯 今日自检清单

- [ ] Docker 基础概念
- [ ] Dockerfile 编写
- [ ] Docker Compose 使用
- [ ] 电商系统容器化
- [ ] 镜像优化

---

## 下一步

明天我们将学习：**CI/CD 持续集成与部署**
