# docker安装
## 安装docker
[Docker CE镜像-Docker CE镜像下载安装-开源镜像站-阿里云](https://developer.aliyun.com/mirror/docker-ce?spm=a2c6h.13651102.0.0.57e31b11wBe9zu)

## 一键配置国内镜像
[1ms-helper - 毫秒镜像配置助手 - 毫秒镜像 - 迷你文档](https://mdoc.cc/mliev/1ms/v1.0.0/71)

## 安装docker compose
[安装Docker-Compose - 毫秒镜像 - 迷你文档](https://mdoc.cc/mliev/1ms/v1.0.0/8)
# docker使用
Docker 命令是容器化操作的核心。以下是 **Docker 命令大全**，按功能分类，包含最常用的操作和参数。

## 📦 镜像管理 (Images)

### 镜像搜索与拉取

```
# 搜索镜像
docker search nginx
docker search --filter=is-official=true nginx  # 只搜官方镜像

# 拉取镜像（优先使用国内镜像源）
docker pull nginx:alpine                      # 拉取指定版本
docker pull nginx                             # 拉取最新版(latest)
docker pull ubuntu:20.04                      # 拉取指定系统版本

# 配置国内镜像加速（解决拉取慢问题）
# 编辑 /etc/docker/daemon.json
{
  "registry-mirrors": [
    "https://docker.1ms.run",                # 毫秒镜像
    "https://hub-mirror.c.163.com",          # 网易
    "https://mirror.baidubce.com"            # 百度云
  ]
}
```

### 镜像查看与管理

```
# 查看镜像列表
docker images
docker image ls
docker images -a                              # 显示所有镜像（包括中间层）
docker images --digests                       # 显示摘要
docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"

# 删除镜像
docker rmi nginx:alpine                       # 删除指定镜像
docker rmi $(docker images -q)                # 删除所有镜像（危险！）
docker image prune                            # 删除悬空镜像(dangling)
docker image prune -a                         # 删除所有未使用的镜像

# 镜像详细信息
docker image inspect nginx:alpine             # 查看镜像详情
docker history nginx:alpine                   # 查看镜像构建历史
```

### 镜像构建与导出

```
# 构建镜像
docker build -t myapp:latest .                # 从当前目录Dockerfile构建
docker build -t myapp:v1 -f Dockerfile.dev .  # 指定Dockerfile

# 导出导入镜像
docker save myapp:latest > myapp.tar          # 导出为tar文件
docker load < myapp.tar                       # 从tar文件导入

# 导出为压缩文件
docker save myapp:latest | gzip > myapp.tar.gz
```

## 🐳 容器管理 (Containers)

### 容器生命周期

```
# 创建并运行容器
docker run nginx:alpine                        # 前台运行
docker run -d nginx:alpine                     # 后台运行(-d)
docker run --name my-nginx nginx:alpine        # 指定名称

# 常用参数组合
docker run -d --name web -p 8080:80 nginx:alpine
docker run -d --name db -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0

# 启动/停止/重启容器
docker start container_name
docker stop container_name                     # 优雅停止
docker stop -t 30 container_name              # 设置超时时间
docker restart container_name
docker kill container_name                     # 强制停止
```

### 容器状态与信息

```
# 查看容器
docker ps                                     # 运行中的容器
docker ps -a                                  # 所有容器（包括停止的）
docker ps -q                                  # 只显示容器ID
docker ps -l                                  # 显示最近创建的容器
docker ps -s                                  # 显示容器大小
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"

# 容器详细信息
docker inspect container_name                 # 查看容器所有配置
docker inspect -f '{{.NetworkSettings.IPAddress}}' container_name
docker logs container_name                    # 查看日志
docker logs -f container_name                 # 实时查看日志
docker logs --tail 100 container_name         # 查看最后100行
docker logs --since 1h container_name         # 查看最近1小时

# 容器资源使用
docker stats                                  # 实时资源监控
docker top container_name                     # 查看容器内进程
```

### 容器操作

```
# 进入容器
docker exec -it container_name /bin/bash      # 进入bash
docker exec -it container_name /bin/sh        # 进入sh
docker exec -it container_name python         # 进入python交互
docker exec container_name ls -la             # 执行单条命令

# 文件操作
docker cp local_file container_name:/path/    # 复制到容器
docker cp container_name:/file local_path     # 从容器复制
```

### 容器清理

```
# 删除容器
docker rm container_name                       # 删除已停止的容器
docker rm -f container_name                    # 强制删除运行中的容器
docker rm $(docker ps -aq)                     # 删除所有停止的容器
docker container prune                         # 删除所有停止的容器

# 清理所有
docker system prune                            # 清理所有未使用的资源
docker system prune -a                         # 彻底清理（包括未使用的镜像）
docker system df                               # 查看磁盘使用情况
```

## 🔗 网络管理 (Network)

```
# 网络管理
docker network ls                              # 查看网络列表
docker network create mynet                    # 创建网络
docker network connect mynet container_name    # 连接容器到网络
docker network disconnect mynet container_name # 断开连接
docker network inspect bridge                  # 查看网络详情
docker network rm mynet                        # 删除网络
docker network prune                           # 清理未使用的网络
```

## 💾 数据卷管理 (Volumes)

```
# 数据卷管理
docker volume ls                               # 查看数据卷
docker volume create mydata                    # 创建数据卷
docker volume inspect mydata                   # 查看数据卷详情
docker volume rm mydata                        # 删除数据卷
docker volume prune                            # 清理未使用的数据卷

# 使用示例
docker run -v mydata:/data nginx              # 命名卷
docker run -v /host/path:/container/path nginx # 绑定挂载
docker run --mount type=bind,source=/host/path,target=/container/path nginx
```

## 🎯 Docker Compose 命令

```
# 基本命令
docker compose up                             # 创建并启动服务
docker compose up -d                          # 后台运行
docker compose down                           # 停止并移除容器
docker compose down -v                        # 同时删除数据卷
docker compose down --rmi all                  # 同时删除镜像

# 服务管理
docker compose ps                             # 查看服务状态
docker compose logs                           # 查看所有服务日志
docker compose logs -f service_name           # 实时查看指定服务
docker compose exec service_name command      # 在服务中执行命令
docker compose restart service_name           # 重启服务
docker compose rm service_name                 # 删除服务容器

# 构建相关
docker compose build                          # 构建镜像
docker compose build --no-cache               # 构建时不使用缓存
docker compose up --build                     # 重新构建并启动

# 配置检查
docker compose config                         # 验证并查看配置
docker compose config --services              # 显示服务列表
docker compose config --volumes               # 显示数据卷列表
```

## ⚙️ 系统与信息

```
# 系统信息
docker version                                # Docker版本
docker info                                  # 系统级信息
docker system info                           # 系统信息
docker system df                             # 磁盘使用情况

# Docker服务管理
sudo systemctl start docker                  # 启动docker服务
sudo systemctl stop docker                   # 停止docker服务
sudo systemctl restart docker                # 重启docker服务
sudo systemctl status docker                 # 查看服务状态
sudo systemctl enable docker                 # 设置开机自启

# 事件与检查
docker events                                # 查看docker事件流
docker stats                                 # 实时监控所有容器
docker top container_name                    # 容器内进程
```

## 🔧 实用技巧与快捷命令

### 1. 批量操作

```
# 停止所有容器
docker stop $(docker ps -aq)

# 删除所有容器
docker rm $(docker ps -aq)

# 删除所有镜像
docker rmi $(docker images -q)

# 清理系统
docker system prune -af

# 批量打标签
docker images | grep none | awk '{print $3}' | xargs docker rmi
```

### 2. 查看容器资源使用

```
# 查看容器资源限制
docker stats --no-stream
# 查看容器CPU/内存使用率
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

### 3. 容器健康检查

```
# 健康检查命令
docker inspect --format='{{json .State.Health}}' container_name
# 等待容器健康
timeout 300 bash -c 'until docker inspect --format="{{.State.Health.Status}}" container_name | grep healthy; do sleep 1; done'
```

### 4. 网络调试

```
# 查看端口映射
docker port container_name
# 查看容器IP
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name
```

## 🐚 常用别名设置（添加到~/.bashrc或~/.zshrc）

```
# Docker快捷命令
alias dps='docker ps'
alias dpsa='docker ps -a'
alias di='docker images'
alias dip='docker image prune'
alias dcp='docker container prune'
alias dvp='docker volume prune'
alias dsp='docker system prune'
alias dl='docker logs'
alias dlf='docker logs -f'
alias de='docker exec -it'
alias dr='docker run'
alias drm='docker rm'
alias drmi='docker rmi'
alias dst='docker stats'
alias dcu='docker compose up -d'
alias dcd='docker compose down'
alias dcl='docker compose logs -f'
```

## 📊 命令速查表

|功能|命令|说明|
|---|---|---|
|**运行容器**​|`docker run -d -p 80:80 nginx`|后台运行nginx|
|**查看容器**​|`docker ps`或 `docker ps -a`|查看运行/所有容器|
|**查看日志**​|`docker logs -f container`|实时查看日志|
|**进入容器**​|`docker exec -it container bash`|进入容器shell|
|**停止容器**​|`docker stop container`|优雅停止|
|**删除容器**​|`docker rm container`|删除容器|
|**查看镜像**​|`docker images`|镜像列表|
|**拉取镜像**​|`docker pull nginx:alpine`|拉取镜像|
|**删除镜像**​|`docker rmi image`|删除镜像|
|**构建镜像**​|`docker build -t myapp .`|构建镜像|
|**查看信息**​|`docker inspect container`|查看容器详情|
|**数据卷**​|`docker volume create data`|创建数据卷|
|**网络**​|`docker network create net`|创建网络|
|**清理**​|`docker system prune`|清理未使用的资源|

## 🆘 帮助与文档

```
# 获取帮助
docker --help
docker run --help
docker build --help

# 查看具体命令帮助
docker command --help
docker run --help | grep -A5 "port"  # 查找特定参数
```


# **Docker Compose 命令大全**

## 🚀 基础命令

### 启动与停止

```
# 启动所有服务（前台运行，查看日志）
docker compose up

# 后台启动服务（最常用）
docker compose up -d

# 启动指定服务
docker compose up -d nginx mysql

# 停止并移除所有容器、网络
docker compose down

# 停止并移除容器、网络、数据卷（谨慎使用）
docker compose down -v

# 停止并移除容器、网络、数据卷、镜像
docker compose down --rmi all

# 仅停止服务，不删除容器
docker compose stop

# 重启已停止的服务
docker compose start
```

### 服务状态管理

```
# 查看运行状态
docker compose ps

# 查看所有服务状态（包括停止的）
docker compose ps -a

# 查看服务资源使用
docker compose top

# 查看服务日志
docker compose logs

# 实时查看日志（跟随输出）
docker compose logs -f

# 查看指定服务日志
docker compose logs -f nginx

# 查看最后N行日志
docker compose logs --tail 100

# 查看最近时间段的日志
docker compose logs --since 1h
```

## ⚙️ 服务操作

### 容器内操作

```
# 进入容器shell（交互式终端）
docker compose exec nginx sh
docker compose exec app bash

# 在容器内执行单条命令
docker compose exec app ls -la
docker compose exec db mysql -u root -p

# 以root用户进入
docker compose exec -u root nginx sh

# 传递环境变量
docker compose exec -e DEBUG=true app python manage.py shell
```

### 服务管理

```
# 重启服务
docker compose restart nginx

# 重启所有服务
docker compose restart

# 强制重启（先stop再start）
docker compose restart --force

# 暂停服务（暂停进程）
docker compose pause nginx

# 恢复服务
docker compose unpause nginx

# 删除已停止的服务容器
docker compose rm nginx

# 强制删除（包括运行中的）
docker compose rm -f nginx

# 删除时同时删除数据卷
docker compose rm -v nginx
```

## 🔧 构建与配置

### 镜像构建

```
# 构建所有服务的镜像
docker compose build

# 构建指定服务的镜像
docker compose build nginx app

# 构建时不使用缓存
docker compose build --no-cache

# 强制重建镜像（即使没变化）
docker compose build --force-rm

# 构建并启动
docker compose up --build

# 拉取服务的最新镜像
docker compose pull

# 拉取指定服务的镜像
docker compose pull nginx
```

### 配置检查

```
# 验证并显示最终配置
docker compose config

# 仅验证配置（不显示）
docker compose config --quiet

# 显示服务列表
docker compose config --services

# 显示数据卷列表
docker compose config --volumes

# 显示网络列表
docker compose config --networks

# 显示环境变量
docker compose config --hash="*"
```

## 📁 文件与路径

### 配置文件管理

```
# 指定配置文件（解决找不到文件问题）
docker compose -f docker-compose.yml up
docker compose -f docker-compose.prod.yml up

# 指定多个配置文件（配置覆盖）
docker compose -f docker-compose.yml -f docker-compose.override.yml up

# 指定项目目录
docker compose -p myproject up
docker compose --project-directory /path/to/project up

# 使用环境变量指定文件
export COMPOSE_FILE=docker-compose.prod.yml
docker compose up
```

### 环境变量配置

```
# 指定环境变量文件
docker compose --env-file .env.production up

# 覆盖环境变量
docker compose up -e DB_HOST=localhost -e DB_PORT=3306

# 查看当前环境变量
docker compose config --environment
```

## 🔍 调试与诊断

### 状态检查

```
# 检查服务健康状态
docker compose ps --format "table {{.Name}}\t{{.Service}}\t{{.State}}\t{{.Ports}}"

# 查看服务依赖关系
docker compose config --services | xargs -I {} sh -c 'echo "{}:" && docker compose config --format json | jq ".services.{}.depends_on"'

# 查看服务镜像信息
docker compose images

# 查看服务端口映射
docker compose port nginx 80
```

### 性能监控

```
# 实时监控资源使用
docker compose stats

# 监控指定服务
docker compose stats nginx mysql

# 格式化输出
docker compose stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

## 🧹 清理与维护

### 资源清理

```
# 删除所有停止的容器
docker compose rm -f

# 删除未使用的镜像
docker compose images -q | xargs docker rmi -f

# 清理构建缓存
docker builder prune

# 清理所有未使用的资源
docker system prune -f

# 查看磁盘使用
docker system df
```

### 数据管理

```
# 创建数据卷
docker volume create db_data

# 查看数据卷
docker volume ls

# 删除未使用的数据卷
docker volume prune

# 备份数据卷
docker run --rm -v db_data:/data -v $(pwd):/backup alpine tar czf /backup/backup.tar.gz /data
```

## 🚀 高级用法

### 扩展与覆盖

```
# 使用扩展文件（多环境配置）
docker compose -f docker-compose.yml -f docker-compose.prod.yml up

# 查看最终合并的配置
docker compose -f docker-compose.yml -f docker-compose.override.yml config

# 创建配置模板
docker compose convert > docker-compose.full.yml
```

### 性能优化参数

```
# 限制资源使用
docker compose up -d --compatibility

# 设置并行操作数
docker compose up --parallel 3

# 超时设置
docker compose up --timeout 60
```

## 📊 常用命令组合

### 开发环境

```
# 开发环境完整流程
docker compose up -d          # 启动服务
docker compose logs -f app    # 查看应用日志
docker compose exec app bash  # 进入应用容器
docker compose down           # 停止服务
```

### 生产环境

```
# 生产环境部署
docker compose -f docker-compose.prod.yml pull    # 拉取最新镜像
docker compose -f docker-compose.prod.yml up -d   # 启动服务
docker compose -f docker-compose.prod.yml logs -f # 监控日志
```

### 调试排错

```
# 排错流程
docker compose ps              # 查看状态
docker compose logs --tail 50  # 查看最近日志
docker compose exec app sh     # 进入容器检查
docker compose restart app     # 重启服务
```

## ⚡ 实用技巧

### 1. 快速重启单个服务

```
# 重启并重建单个服务
docker compose up -d --no-deps --build nginx

# 强制重建单个服务
docker compose build --no-cache nginx && docker compose up -d nginx
```

### 2. 环境变量管理

```
# 创建环境文件
cat > .env << EOF
DB_HOST=mysql
DB_PORT=3306
REDIS_URL=redis://redis:6379
EOF

# 使用不同环境文件
docker compose --env-file .env.production up
```

### 3. 网络配置

```
# 查看网络配置
docker network ls
docker network inspect project_default

# 连接外部容器到compose网络
docker network connect project_default external_container
```

### 4. 国内镜像加速

```
# 在docker-compose.yml中直接使用国内镜像
services:
  nginx:
    image: registry.cn-hangzhou.aliyuncs.com/library/nginx:alpine
    
  # 或者使用镜像加速器
  app:
    build: .
    image: myapp:latest
    # 构建时使用国内源
    # 在Dockerfile中：RUN sed -i 's/deb.debian.org/mirrors.aliyun.com/g' /etc/apt/sources.list
```

## 🎯 命令速查表

|场景|命令|说明|
|---|---|---|
|**启动服务**​|`docker compose up -d`|后台启动所有服务|
|**停止服务**​|`docker compose down`|停止并清理|
|**查看状态**​|`docker compose ps`|服务运行状态|
|**查看日志**​|`docker compose logs -f`|实时日志|
|**进入容器**​|`docker compose exec app sh`|进入容器shell|
|**重启服务**​|`docker compose restart`|重启所有服务|
|**构建镜像**​|`docker compose build`|构建服务镜像|
|**拉取镜像**​|`docker compose pull`|拉取最新镜像|
|**配置检查**​|`docker compose config`|验证配置文件|
|**指定文件**​|`docker compose -f prod.yml up`|使用指定配置文件|

## 📝 最佳实践建议

1. **版本管理**：始终在`docker-compose.yml`开头指定版本
    
    ```
    version: '3.8'
    ```
    
2. **服务命名**：使用有意义的服务名称
    
    ```
    services:
      web-server:
        image: nginx:alpine
      database:
        image: mysql:8.0
    ```
    
3. **环境分离**：使用不同配置文件
    
    ```
    docker-compose.yml          # 基础配置
    docker-compose.dev.yml      # 开发环境覆盖
    docker-compose.prod.yml     # 生产环境覆盖
    ```
    
4. **数据持久化**：使用命名卷
    
    ```
    volumes:
      db_data:
      app_logs:
    ```
    
5. **网络隔离**：使用自定义网络
    
    ```
    networks:
      app-network:
        driver: bridge
    ```
    

## 🆘 常见问题解决

### Q1: 命令找不到或报错？

```
# 检查是否安装正确
docker compose version

# 如果是v1版本，命令是 docker-compose（有横线）
docker-compose --version

# 安装v2版本
sudo apt install docker-compose-plugin  # Ubuntu/Debian
```

### Q2: 配置文件找不到？

```
# 确保在正确目录
pwd
ls -la docker-compose*

# 使用绝对路径
docker compose -f /full/path/to/docker-compose.yml up
```

### Q3: 端口冲突？

```
# 查看端口占用
sudo netstat -tlnp | grep :80

# 修改端口映射
# 在docker-compose.yml中：
ports:
  - "8080:80"  # 主机端口:容器端口
```

### Q4: 国内拉取镜像慢？

```
# 方法1：配置Docker镜像加速器
# 方法2：在compose文件中使用国内镜像
# 注意，阿里云的镜像源有限制
image: registry.cn-hangzhou.aliyuncs.com/library/nginx:alpine

# 方法3：提前拉取镜像
docker pull nginx:alpine
docker compose up -d
```


