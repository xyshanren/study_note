Dockerfile

```dockerfile
FROM ubuntu:18.04

MAINTAINER Dockerfiles

# 安装git、python、nginx、supervisor等，并清理缓存
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y \
        vim \
        git \
        python3 \
        python3-dev \
        python3-setuptools \
        python3-pip \
        nginx \
        supervisor \
        libmysqlclient-dev && \
        pip3 install --upgrade -i https://pypi.doubanio.com/simple/ pip setuptools && \
        rm -rf /var/lib/apt/lists/*

RUN pip3 install -i https://pypi.doubanio.com/simple/ uwsgi

# 环境变量
# django项目的settings.py中的变量
ENV MYSQL_DATABASE_NAME mysite_db
ENV EMAIL_HOST_USER leexc_2014@foxmail.com
# 邮箱的密码或者授权码（需要替换成自己的）
ENV EMAIL_HOST_PASSWORD xxxxx

# nginx、supervisor配置
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
COPY nginx-app.conf /etc/nginx/sites-available/default
COPY supervisor-app.conf /etc/supervisor/conf.d/

# 安装项目所需python第三方库
COPY requirements.txt /home/docker/code/mysite/
RUN pip3 install -i https://pypi.doubanio.com/simple/ \
                 -r /home/docker/code/mysite/requirements.txt

# uwsgi.ini 及 uwsgi_params
COPY . /home/docker/code/

EXPOSE 80
CMD ["supervisord", "-n"]
```



supervisor-app.conf

```shell
[program:app-uwsgi]
command = /usr/local/bin/uwsgi --ini /home/docker/code/uwsgi.ini

[program:nginx-app]
command = /usr/sbin/nginx
```



uwsgi.ini

```shell
[uwsgi]
chdir = /home/docker/code/mysite
module = mysite.wsgi
master = true
processes = 4
socket = 127.0.0.1:8000

http-socket 0.0.0.0:8000
vacuum = true
```



nginx-app.conf

```nginx
#负载集群
upstream django {
	server 127.0.0.1:8000;
}

server {
	listen      80;
	server_name 172.17.50.95;
	charset     utf-8;
	# max upload size
	client_max_body_size 75M;

	location / {
		# nginx自带的ngx_http_uwsgi_module模块，起到nginx和uwsgi交互的作用
		# 通过uwsgi_pass指定服务器地址和协议，将动态请求转发给uwsgi处理
		# django是上面定义的负载集群
		uwsgi_pass django;
		# 指定uwsgi_params
		include uwsgi_params;
	}
	# nginx处理静态页面资源（static_collected 为收集完静态资源后的目录）
	location /static {
		alias /home/docker/code/mysite/static/;
         #alias /home/docker/code/mysite/static_collected/;
	}
	# nginx处理媒体资源
	location /media {
		alias /home/docker/code/mysite/media/;
	}
    # 配置网站图标文件
    location /favicon.ico {
        alias /home/docker/code/mysite/favicon.ico;
    }
}
```

使用shell脚本持续部署

start.sh

```shell
#!/bin/bash

# 镜像不存在时创建镜像
if ! docker images | grep mysite; then
    echo 'The docker image does not exist,'
    echo 'Now creating image <mysite>...'
    docker build -t mysite $(pwd)
fi

# 镜像存在时，检查容器是否存在
if docker ps -a | grep -i mysite; then
    # 容器存在时则删除容器
    echo 'The docker container <mysite> already exist, deleting it...'
    docker container rm -f webapp-mysite
    # 启动容器
    docker run -itd \
               --link mysql:mysql \
               -v /root/git_repo/mysite/:/home/docker/code/mysite \
               --name webapp-mysite \
               -p 80:80 \
           mysite
else
    # 第一次创建容器时需要更新数据库
    # 初始化数据库
    # 收集静态文件
    # 创建django项目的debug.log文件
    # 对media目录设置赋予所有权限
    # 启动Nginx和uwsgi
    docker run -itd \
               --link mysql:mysql \
               -v /root/git_repo/mysite/:/home/docker/code/mysite \
               --name webapp-mysite \
               -p 80:80 \
           mysite \
           sh -c 'python3 /home/docker/code/mysite/manage.py migrate && python3 /home/docker/code/mysite/manage.py createcachetable' \
           sh -c 'python3 /home/docker/code/mysite/manage.py collectstatic' \
           sh -c 'touch /home/docker/code/mysite_debug.log && chmod 666 /home/docker/code/mysite_debug.log' \
           sh -c 'chmod 777 /home/docker/code/mysite/media' \
           sh -c 'supervisord -c /etc/supervisor/supervisord.conf'
fi
```





创建镜像

docker build -t mysite /root/docker/django-uwsgi-nginx

启动容器

docker run -itd --link mysql:mysql -v /root/git_repo/mysite/:/home/docker/code/mysite --name webapp-mysite -p 80:80 mysite bash

进入容器

docker exec -it webapp-mysite bash





初始化数据库

python3 /home/docker/code/mysite/manage.py migrate

创建缓存表

python3 /home/docker/code/mysite/manage.py createcachetable

启动项目

~~supervisord -n~~

通过配置文件启动supervisor

supervisord -c /etc/supervisor/supervisord.conf

查看任务状态

supervisorctl status



参考链接：https://zhuanlan.zhihu.com/p/29609591



需要在容器中执行的命令

初始化模型迁移文件到数据库中

python3 /home/docker/code/mysite/manage.py migrate 

创建缓存表

python3 /home/docker/code/mysite/manage.py createcachetable

收集静态文件

python3 /home/docker/code/mysite/manage.py collectstatic



创建django项目的debug.log文件

touch /home/docker/code/mysite_debug.log

并赋予写的权限

chmod 666 /home/docker/code/mysite_debug.log



对media目录设置赋予所有权限

chmod 777 /home/docker/code/mysite/media







