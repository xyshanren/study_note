# 使用三容器部署Django #

## Docker的安装与使用 ##

>详情请参照：<https://yeasy.gitbook.io/docker_practice/>

### 安装docker ###

- 卸载旧版本
  旧版本的 Docker 称为 docker 或者 docker-engine，使用以下命令卸载旧版本：

    ```shell
    sudo yum remove docker \
                    docker-client \
                    docker-client-latest \
                    docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-selinux \
                    docker-engine-selinux \
                    docker-engine
    ```

- 安装依赖包

    ```shell
    sudo yum install -y yum-utils
    ```

- 执行下面的命令添加 yum 软件源：  

    ```shell
    # 鉴于国内网络问题，这里使用国内源，官方源请在注释中查看
    sudo yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

    sudo sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo

    # 官方源
    # sudo yum-config-manager \
    #   --add-repo \
    #   https://download.docker.com/linux/centos/docker-ce.repo
    ```

- 更新 yum 软件源缓存，并安装 docker-ce

    ```shell
    sudo yum install docker-ce docker-ce-cli containerd.io
    ```

- CentOS8 额外设置  
  由于 CentOS8 防火墙使用了 nftables，但 Docker 尚未支持 nftables，我们可以使用如下设置使用 iptables：  
  更改 /etc/firewalld/firewalld.conf  

    ```shell
    # FirewallBackend=nftables
    FirewallBackend=iptables
    ```

  或者执行如下命令：

    ```shell
    firewall-cmd --permanent --zone=trusted --add-interface=docker0

    firewall-cmd --reload
    ```

- 启动 Docker  

    ```shell
    sudo systemctl enable docker
    sudo systemctl start docker
    ```

- 测试 Docker 是否安装正确  

```shell
docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

>若能正常输出以上信息，则说明安装成功。

### 安装docker-compose ###

```shell
# Step 1: 下载docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
## 快速下载使用这个地址
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.26.1/docker-compose-$(uname -s)-$(uname -m)" -o  /usr/local/bin/docker-compose

# Step 2: 给予docker-compose可执行权限
sudo chmod +x /usr/local/bin/docker-compose

# Step 3: 查看docker-compose版本
docker-compose --version

# 卸载
sudo rm /usr/local/bin/docker-compose
```

## 部署详情 ##

### 准备 ###

- Django + Uwsgi + Nginx + MySQL组合容器示意图
  - Django + Uwsgi容器：核心应用程序，处理动态请求
  - MySQL 容器：数据库服务
  - Nginx容器：反向代理服务并处理静态资源请求

- Docker-compose部署Django项目布局树形图

   ```txt
   ├── compose # 存放各项容器服务的Dockerfile和配置文件
   │   ├── mysql
   │   │   ├── conf
   │   │   │   └── my.cnf # MySQL配置文件
   │   │   └── init
   │   │   │   └── init.sql # MySQL启动脚本
   │   ├── nginx
   │   │   ├── Dockerfile # 构建Nginx镜像所的Dockerfile
   │   │   ├── log # 挂载保存nginx容器内nginx日志
   │   │   ├── nginx.conf # Nginx配置文件
   │   │   └── ssl # 如果需要配置https需要用到
   │   └── uwsgi # 挂载保存django+uwsgi容器内uwsgi日志
   ├── docker-compose.yml # 核心编排文件
   ├── mysite # 常规Django项目目录
   │   ├── Dockerfile # 构建Django+Uwsgi镜像的Dockerfile
   │   ├── apps # 存放Django项目的各个apps
   │   ├── manage.py
   │   ├── mysite # Django项目配置文件
   │   │   ├── asgi.py
   │   │   ├── __init__.py
   │   │   ├── settings.py
   │   │   ├── urls.py
   │   │   └── wsgi.py
   │   ├── pip.conf # 非必需。pypi源设置成国内，加速pip安装
   │   ├── requirements.txt # Django项目依赖文件
   │   ├── start.sh # 启动Django+Uwsgi容器后要执行的脚本
   │   ├── media # 用户上传的媒体资源与静态文件
   │   ├── static # 项目所使用到的静态文件
   │   └── uwsgi.ini # uwsgi配置文件
   ├── log
   │   ├── mysite
   │   │   ├── mysite_debug.log # django日志
   ```

> 注意：这里需要给 mysite_debug.log 赋写权限 chmod +666 mysite_debug.log

### 开始部署 ###

- 第一步：编写docker-compose.yml文件

  ```yaml
  version: "3"
  
  volumes: # 自定义数据卷，位于宿主机/var/lib/docker/volumes内
   mysite_db_vol: # 定义数据卷同步容器内mysql数据
   mysite_media_vol: # 定义数据卷同步media文件夹数据
  
  services:
   db:
     image: mysql:5.7
     environment:
        - MYSQL_ROOT_PASSWORD=123456 # 数据库密码
        - MYSQL_DATABASE=mysite # 数据库名称
        - MYSQL_USER=dbuser # 数据库用户名
        - MYSQL_PASSWORD=password # 用户密码
  
     volumes:
        - mysite_db_vol:/var/lib/mysql:rw # 挂载数据库数据, 可读可写
        - ./compose/mysql/conf/my.cnf:/etc/mysql/my.cnf # 挂载配置文件
        - ./compose/mysql/init:/docker-entrypoint-initdb.d/ # 挂载数据初始化sql脚本
     ports:
        - "3306:3306" # 与配置文件保持一致
      restart: always
  
   web:
     build: ./mysite # 使用mysite目录下的Dockerfile
     expose:
        - "8000"
     volumes:
        - ./mysite:/var/www/html/mysite # 挂载项目代码
        - mysite_media_vol:/var/www/html/mysite/media # 以数据卷挂载容器内用户上传媒体文件
        - ./compose/uwsgi:/tmp # 挂载uwsgi日志
        - ./log/mysite/mysite_debug.log:/var/www/html/log/mysite_debug.log # 挂载Django的日志
     links:
        - db
     depends_on: # 依赖关系
        - db
     environment:
        - DEBUG=False
      restart: always
     tty: true
     stdin_open: true
  
   nginx:
     build: ./compose/nginx
     ports:
        - "80:80"
        - "443:443"
     expose:
        - "80"
        - "443"
     volumes:
        - ./mysite/static_collected:/usr/share/nginx/html/static_collected # 挂载静态文件
        - ./mysite/favicon.ico:/usr/share/nginx/html/favicon.ico # 挂载图标
        - ./compose/nginx/ssl:/usr/share/nginx/ssl # 挂载ssl证书目录
        - ./compose/nginx/log:/var/log/nginx # 挂载日志
        - mysite_media_vol:/usr/share/nginx/html/media # 挂载用户上传媒体文件
     links:
        - web
     depends_on:
      - web
      restart: always
  ```

- 第二步：编写Web (Django+Uwsgi)镜像和容器所需文件

  构建Web镜像所使用的Dockerfile

  ```dockerfile
  # mysite/Dockerfile
  # 建立 python3.7 环境
  FROM python:3.7
  
  # 镜像作者大江狗
  MAINTAINER DJG
  
  # 设置 python 环境变量
  ENV PYTHONUNBUFFERED 1
  # django项目的settings.py中的变量
  ENV SECRET_KEY xxx
  ENV MYSQL_DATABASE_NAME mysite_db
  ENV MYSQL_USER mysite
  ENV MYSQL_USER_PASSWORD xxxx
  ENV EMAIL_HOST_USER xxxx
  # 邮箱的密码或者授权码（需要替换成自己的）
  ENV EMAIL_HOST_PASSWORD xxxxx
  
  COPY pip.conf /root/.pip/pip.conf
  
  # 创建 mysite 文件夹
  RUN mkdir -p /var/www/html/mysite
  
  # 创建 django的日志文件目录
  RUN mkdir -p /var/www/html/log
  
  # 将 mysite 文件夹为工作目录
  WORKDIR /var/www/html/mysite
  
  # 将当前目录加入到工作目录中（. 表示当前目录）
  ADD . /var/www/html/mysite
  
  # 更新pip版本
  RUN /usr/local/bin/python -m pip install --upgrade pip
  
  # 利用 pip 安装依赖
  RUN pip install -r requirements.txt
  
  # 去除windows系统编辑文件中多余的\r回车空格
  RUN sed -i 's/\r//' ./start.sh
  
  # 给start.sh可执行权限
  RUN chmod +x ./start.sh
  ```

  本Django项目所依赖的requirements.txt内容如下所示：

  ```shell
  # django
  Django==2.2.1
  # uwsgi
  uwsgi==2.0.18
  # editor
  django-ckeditor==5.9.0
  django-js-asset==1.2.2
  # mysql
  mysqlclient==1.4.6
  # for images
  Pillow==6.1.0
  # for sql
  sqlparse==0.3.0
  # for time
  pytz==2019.2
  # 装饰器
  wrapt==1.12.1
  ```

  将pypi源设置成国内，加速pip安装，pip.conf配置如下：

  ```shell
  [global]
  index-url = https://pypi.tuna.tsinghua.edu.cn/simple
  ```

  

  start.sh脚本文件内容如下所示。最重要的是最后一句，使用uwsgi.ini配置文件启动Django服务。

  ```shell
  #!/bin/bash
  # 从第一行到最后一行分别表示：
  # 1. 收集静态文件到根目录，(已使用static_collected作为静态目录)
  # 2. 生成数据库可执行文件，
  # 3. 根据数据库可执行文件来修改数据库
  # 4. 创建缓存表
  # 5. 用 uwsgi启动 django 服务
  # python manage.py collectstatic --noinput&&
  python manage.py makemigrations&&
  python manage.py migrate&&
  python manage.py createcachetable&&
  uwsgi --ini /var/www/html/mysite/uwsgi.ini
  ```

  uwsgi.ini配置文件如下所示：

  ```shell
  [uwsgi]
  
  project=mysite
  uid=www-data
  gid=www-data
  base=/var/www/html
  
  chdir=%(base)/%(project)
  module=%(project).wsgi:application
  master=True
  processes=2
  
  socket=0.0.0.0:8000
  chown-socket=%(uid):www-data
  chmod-socket=664
  
  vacuum=True
  max-requests=5000
  
  pidfile=/tmp/%(project)-master.pid
  daemonize=/tmp/%(project)-uwsgi.log
  
  #设置一个请求的超时时间(秒)，如果一个请求超过了这个时间，则请求被丢弃
  harakiri = 60
  post buffering = 8192
  buffer-size= 65535
  #当一个请求被harakiri杀掉会，会输出一条日志
  harakiri-verbose = true
  
  #开启内存使用情况报告
  memory-report = true
  
  #设置平滑的重启（直到处理完接收到的请求）的长等待时间(秒)
  reload-mercy = 10
  
  #设置工作进程使用虚拟内存超过N MB就回收重启
  reload-on-as= 1024
  python-autoreload=1
  ```

- 第三步：编写Nginx镜像和容器所需文件

  构建Nginx镜像所使用的Dockerfile如下所示：

  ```dockerfile
  # nginx镜像compose/nginx/Dockerfile
  
  FROM nginx:latest
  
  # 删除原有配置文件，创建静态资源文件夹和ssl证书保存文件夹
  # RUN rm /etc/nginx/conf.d/default.conf \
  RUN mkdir -p /usr/share/nginx/html/static_collected \
  && mkdir -p /usr/share/nginx/html/media \
  && mkdir -p /usr/share/nginx/ssl
  
  # 设置Media文件夹用户和用户组为Linux默认www-data, 并给予可读和可执行权限,
  # 否则用户上传的图片无法正确显示。
  RUN chown -R www-data:www-data /usr/share/nginx/html/media \
  && chmod -R 775 /usr/share/nginx/html/media
  
  # 添加配置文件
  ADD ./nginx.conf /etc/nginx/conf.d/
  
  # 关闭守护模式
  CMD ["nginx", "-g", "daemon off;"]
  ```

  Nginx的配置文件如下所示

  ```nginx
  # nginx配置文件
  # compose/nginx/nginx.conf
  
  upstream django {
     ip_hash;
     server web:8000; # Docker-compose web服务端口
  }
  # 监听80端口，并转发给https
  server {
     listen 80;
     server_name questlab.xyz;
     return  301 https://$server_name$request_uri;
  }
  
  server {
     listen 443 ssl; # 监听443端口，https使用的
     server_name 127.0.0.1; # 可以是nginx容器所在ip地址或127.0.0.1，不能写宿主机外网ip地址
     charset utf-8;
  
     ssl_certificate /usr/share/nginx/ssl/questlab.xyz.pem;
     ssl_certificate_key /usr/share/nginx/ssl/questlab.xyz.key;
     ssl_session_timeout 5m;
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;  #使用此加密套件。
     ssl_protocols TLSv1 TLSv1.1 TLSv1.2;   #使用该协议进行配置。
     ssl_prefer_server_ciphers on;
     
     client_max_body_size 10M; # 限制用户上传文件大小
  
     location /static {
         alias /usr/share/nginx/html/static_collected; # 静态资源路径
     }
  
     location /media {
         alias /usr/share/nginx/html/media; # 媒体资源，用户上传文件路径
     }
  
     location / {
         include /etc/nginx/uwsgi_params;
         uwsgi_pass django;
         uwsgi_read_timeout 600;
         uwsgi_connect_timeout 600;
         uwsgi_send_timeout 600;
  
         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_set_header Host $http_host;
         proxy_redirect off;
         proxy_set_header X-Real-IP  $remote_addr;
         # proxy_pass http://django; # 使用uwsgi通信，而不是http，所以不使用proxy_pass。
     }
  }
  
     access_log /var/log/nginx/access.log main;
     error_log /var/log/nginx/error.log warn;
  
     server_tokens off;
  ```

- 第四步：编写DB (MySQL)容器配置文件

  给MySQL增加配置文件

  ```shell
  # compose/mysql/conf/my.cnf
  [mysqld]
  user=mysql
  default-storage-engine=INNODB
  character-set-server=utf8
  
  port            = 3306 # 端口与docker-compose里映射端口保持一致
  #bind-address = localhost #一定要注释掉，mysql所在容器和django所在容器不同IP
  
  basedir         = /usr
  datadir         = /var/lib/mysql
  tmpdir          = /tmp
  pid-file        = /var/run/mysqld/mysqld.pid
  socket          = /var/run/mysqld/mysqld.sock
  skip-name-resolve  # 这个参数是禁止域名解析的，远程访问推荐开启skip_name_resolve。
  
  [client]
  port = 3306
  default-character-set=utf8
  
  [mysql]
  no-auto-rehash
  default-character-set=utf8
  ```

  还需设置MySQL服务启动时需要执行的脚本命令, 注意这里的用户名和password必需和docker-compose.yml里与MySQL相关的环境变量保持一致。

  ```sql
  # compose/mysql/init/init.sql
  GRANT ALL PRIVILEGES ON *.* TO dbuser@"%" IDENTIFIED BY "password";
  FLUSH PRIVILEGES;
  ```

- 第五步：修改Django项目settings.py

  在好docker-compose.yml并编排好各容器的Dockerfile及配置文件后，先不要急于使用Docker-compose命令构建镜像和启动容器。还有一件非常重要的事情要做，修改Django的settings.py, 提供mysql服务的配置信息。最重要的几项配置如下所示：

  ```python
   # 生产环境设置 Debug = False
   Debug = False
   
   # 设置ALLOWED HOSTS
   ALLOWED_HOSTS = ['your_server_IP', 'your_domain_name']
   
   # 设置STATIC ROOT 和 STATIC URL
   STATIC_ROOT = os.path.join(BASE_DIR, 'static')
   STATIC_URL = "/static/"
   
   # 设置MEDIA ROOT 和 MEDIA URL
   MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
   MEDIA_URL = "/media/"
   
   # 设置数据库。这里用户名和密码必需和docker-compose.yml里mysql环境变量保持一致
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'mysite', # 数据库名
           'USER':'dbuser', # 你设置的用户名 - 非root用户
           'PASSWORD':'password', # # 换成你自己密码
           'HOST': 'db', # 注意：这里使用的是db别名，docker会自动解析成ip
           'PORT':'3306', # 端口
      }
   }
  ```

- 第六步：使用docker-compose 构建镜像并启动容器组服务

  命令如下：

  ```shell
  # 进入docker-compose.yml所在文件夹，输入以下命令构建镜像
  docker-compose build
  # 查看已生成的镜像
  docker images
  # 启动容器组服务
  docker-compose up(start|stop)
  # 查看运行中的容器
  docker ps
  ```

- 第七步：进入web容器内执行Django命令并启动uwsgi服务器

  执行Django相关命令并启动uwsgi服务器。
  进入web容器执行我们先前编写的脚本文件start.sh即可：

  ```shell
  docker exec -it mysite_docker_web_1 bash start.sh
  ```

> 该记录参照：<https://zhuanlan.zhihu.com/p/145364353>