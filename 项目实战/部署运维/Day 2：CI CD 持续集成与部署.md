> 学习日期：2026-05-20（周二）

## 学习目标

1. 理解 CI/CD 概念
2. 掌握 GitHub Actions 使用
3. 能够实现自动化部署

---

## 1. CI/CD 基础

### 1.1 什么是 CI/CD

| 概念 | 说明 |
|------|------|
| CI (持续集成) | 自动化构建和测试，每次代码提交触发 |
| CD (持续交付) | 自动将代码部署到测试/预生产环境 |
| CD (持续部署) | 自动将代码部署到生产环境 |

### 1.2 工作流程

```
代码提交 → 构建 → 测试 → 部署
    ↓       ↓      ↓      ↓
  Git    Docker  单元测试  服务器
  Push   镜像   集成测试  K8s/Docker
```

---

## 2. GitHub Actions

### 2.1 基础配置

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Build
        run: npm run build
```

### 2.2 多环境配置

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}
      
      - name: Build and push
        uses: docker/build-push-action@v4
        with:
          context: .
          push: true
          tags: username/myapp:latest
      
      - name: Deploy to server
        uses: appleboy/ssh-action@v0.1.0
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          script: |
            cd /opt/myapp
            docker-compose pull
            docker-compose up -d
```

---

## 3. 后端 CI/CD 配置

### 3.1 后端工作流

```yaml
# .github/workflows/backend-ci.yml
name: Backend CI/CD

on:
  push:
    branches: [ main ]
    paths:
      - 'backend/**'
  pull_request:
    branches: [ main ]
    paths:
      - 'backend/**'

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
      
      redis:
        image: redis:7
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 6379:6379

    steps:
      - uses: actions/checkout@v3
      
      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Cache pip
        uses: actions/cache@v3
        with:
          path: ~/.cache/pip
          key: ${{ runner.os }}-pip-${{ hashFiles('backend/requirements.txt') }}
          restore-keys: |
            ${{ runner.os }}-pip-
      
      - name: Install dependencies
        working-directory: backend
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest pytest-cov
      
      - name: Run tests
        working-directory: backend
        run: |
          pytest --cov=app --cov-report=xml
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/test
          REDIS_URL: redis://localhost:6379
      
      - name: Build Docker image
        working-directory: backend
        run: |
          docker build -t myapp-backend:${{ github.sha }} .
      
      - name: Push to Registry
        if: github.ref == 'refs/heads/main'
        run: |
          docker tag myapp-backend:${{ github.sha }} username/myapp-backend:latest
          docker push username/myapp-backend:latest
```

---

## 4. 前端 CI/CD 配置

### 4.1 前端工作流

```yaml
# .github/workflows/frontend-ci.yml
name: Frontend CI/CD

on:
  push:
    branches: [ main ]
    paths:
      - 'frontend/**'
  pull_request:
    branches: [ main ]
    paths:
      - 'frontend/**'

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
          cache-dependency-path: frontend/package-lock.json
      
      - name: Install dependencies
        working-directory: frontend
        run: npm ci
      
      - name: Lint
        working-directory: frontend
        run: npm run lint
      
      - name: Run tests
        working-directory: frontend
        run: npm run test:coverage
      
      - name: Build
        working-directory: frontend
        run: npm run build

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install and Build
        working-directory: frontend
        run: |
          npm ci
          npm run build
      
      - name: Deploy to Firebase
        uses: w9jds/firebase-action@v2.2.0
        with:
          args: deploy --only hosting
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```

---

## 5. 自动化部署

### 5.1 服务器部署脚本

```bash
#!/bin/bash
# deploy.sh

# 拉取最新代码
cd /opt/myapp
git pull origin main

# 拉取最新镜像
docker-compose pull

# 重启服务
docker-compose up -d --build

# 清理旧镜像
docker image prune -f

# 查看日志
docker-compose logs -f
```

### 5.2 Docker Compose 优化

```yaml
# docker-compose.prod.yml
version: '3.8'

services:
  backend:
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  frontend:
    restart: always
```

### 5.3 健康检查

```python
# backend/app/health.py
from fastapi import APIRouter

router = APIRouter()

@router.get("/health")
def health_check():
    return {"status": "healthy"}
```

---

## 6. 监控与日志

### 6.1 日志配置

```yaml
# docker-compose.yml
services:
  backend:
    logging:
      driver: "json-file"
      options:
        max-size: "100m"
        max-file: "5"
```

### 6.2 Prometheus 配置

```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'backend'
    static_configs:
      - targets: ['backend:8000']
```

---

## 7. 完整的 CI/CD 流程

### 7.1 开发流程

```
1. 开发功能
   ↓
2. 提交代码到 develop 分支
   ↓
3. GitHub Actions 自动：
   - 安装依赖
   - 运行单元测试
   - 运行集成测试
   - 构建 Docker 镜像
   ↓
4. 部署到测试环境
   ↓
5. 合并到 main 分支
   ↓
6. GitHub Actions 自动：
   - 构建生产镜像
   - 推送到镜像仓库
   - 部署到生产环境
```

### 7.2 分支策略

```
main (生产)
  ↑
develop (开发)
  ↑
feature/xxx (功能分支)
  ↑
bugfix/xxx (修复分支)
```

---

## 📝 课后练习

```yaml
# ========== 练习 1：CI 配置 ==========
# 1. 创建 GitHub Actions 工作流
# 2. 配置自动化测试

# ========== 练习 2：CD 配置 ==========
# 1. 配置 Docker 镜像构建
# 2. 配置自动部署

# ========== 练习 3：完整流程 ==========
# 1. 配置多环境部署
# 2. 配置回滚机制
```

---

## 🎯 今日自检清单

- [ ] CI/CD 基础概念
- [ ] GitHub Actions 基础
- [ ] 后端 CI/CD 配置
- [ ] 前端 CI/CD 配置
- [ ] 自动化部署
- [ ] 监控与日志

---

## 部署与运维总结

本周我们完成了部署与运维的学习：

1. ✅ Day 1: Docker 容器化部署
2. ✅ Day 2: CI/CD 持续集成与部署

---

## 🎉 全栈学习路线完成！

**📊 最终学习进度：**

| 阶段 | 内容 | 状态 |
|------|------|------|
| TypeScript | 8天 | ✅ |
| Vue 3 | 7天 | ✅ |
| Python FastAPI | 5天 | ✅ |
| 鸿蒙 ArkTS | 5天 | ✅ |
| 电商项目 | 5天 | ✅ |
| 部署运维 | 2天 | ✅ |

**总计：32天**

---

你已经完成了全栈开发的学习路线！🎉

接下来可以：
1. 动手做自己的项目
2. 准备面试
3. 参与开源项目

需要任何帮助随时问我！
