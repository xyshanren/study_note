---
title: 使用atlas代理实现MySQL的读写分离
author:
  - 守一
created: 2021-01-12
description: 使用atlas实现MySQL读写分离的部署笔记
tags:
  - mysql
---

# 使用atlas代理实现MySQL的读写分离 #

- [使用atlas代理实现MySQL的读写分离](#使用atlas代理实现mysql的读写分离)
  - [Docker的安装与使用](#docker的安装与使用)
    - [安装docker](#安装docker)
    - [安装docker-compose](#安装docker-compose)
  - [部署详情](#部署详情)
    - [准备](#准备)
    - [开始部署](#开始部署)
    - [使用与维护](#使用与维护)

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

- 使用docker-compose部署MySQL集群及Atlas的布局树形图  

    ```txt
    mysql-cluster # 项目根目录
    ├── Atlas # atlas
    │   ├── conf
    │   │   └── docker-atlas.cnf # atlas的配置文件
    │   └── log # 挂载保存atlas镜像内的日志
    ├── docker-compose.yml # 核心编排文件
    ├── master # 主节点
    │   ├── db # 同步容器内的MySQL数据文件
    │   └── conf
    │   │   └── my.cnf # MySQL主节点配置文件
    ├── slave1 # 从节点1
    │   ├── db # 同步容器内的MySQL数据文件
    │   ├── init
    │   │   └── init.sql # 数据库初始化文件
    │   └── conf
    │   │   └── my.cnf # MySQL配置文件
    ├── slave2 # 从节点2
    │   ├── db # 同步容器内的MySQL数据文件
    │   ├── init
    │   │   └── init.sql # 数据库初始化文件
    │   └── conf
    │   │   └── my.cnf # MySQL配置文件
    ```

### 开始部署 ###

- 第一步：编写 `docker-compose.yml` 文件  

    ```yaml
    version: "3"
    services:
        master:
            image: mysql:5.7
            environment:
                - MYSQL_ROOT_PASSWORD=123456
            ports:
                - "3330:3306"
            volumes:
                - ./master/db:/var/lib/mysql:rw # 挂载数据库数据, 可读可写
                - ./master/conf/my.cnf:/etc/mysql/conf.d/my.cnf # 挂载配置文件

        slave1:
            image: mysql:5.7
            environment:
                - MYSQL_ROOT_PASSWORD=123456
            ports:
                - "3331:3306"
            volumes:
                - ./slave1/db:/var/lib/mysql:rw # 挂载数据库数据, 可读可写
                - ./slave1/conf/my.cnf:/etc/mysql/conf.d/my.cnf # 挂载配置文件
                - ./slave1/init/:/docker-entrypoint-initdb.d/ # 挂载数据初始化sql脚本
            depends_on:
                - master
            links:
                - master

        slave2:
            image: mysql:5.7
            environment:
                - MYSQL_ROOT_PASSWORD=123456
            ports:
                - "3332:3306"
            volumes:
                - ./slave2/db:/var/lib/mysql:rw # 挂载数据库数据, 可读可写
                - ./slave2/conf/my.cnf:/etc/mysql/conf.d/my.cnf # 挂载配置文件
                - ./slave2/init/:/docker-entrypoint-initdb.d/ # 挂载数据初始化sql脚本
            depends_on:
                - master
            links:
                - master

        atlas:
            image: mybbcat/docker-360atlas
            ports:
                - "1234:1234"
                - "2345:2345"
            volumes:
                - /root/svr/mysql-cluster/Atlas/conf/docker-atlas.cnf:/usr/local/mysql-proxy/conf/docker-atlas.cnf # 挂载配置文件
                - /root/svr/mysql-cluster/Atlas/log/:/usr/local/mysql-proxy/log/ # 挂载日志
            depends_on:
                - master
                - slave1
                - slave2
    ```

- 第二步：编写MySQL主节点的配置文件  

    ```shell
    # master/conf/my.cnf
    [mysqld]
    # 实例id，不能和集群中其他实例相同
    server-id=99
    # bin log 文件的前缀
    log-bin=mysql-bin
    # 要同步的数据库
    # binlog-do-db=
    # 要忽略的数据库
    binlog-ignore-db=information_schema
    binlog-ignore-db=mysql
    binlog-ignore-db=personalsite
    binlog-ignore-db=test

    # 设置mysql的安装目录
    basedir         = /usr
    # 设置mysql数据库的数据的存放目录
    datadir         = /var/lib/mysql
    tmpdir          = /tmp
    pid-file        = /var/run/mysqld/mysqld.pid
    socket          = /var/run/mysqld/mysqld.sock

    # 设置默认使用的端口，与docker-compose里映射端口保持一致
    port            = 3306
    #一定要注释掉，主节点所在容器与从节点不同
    #bind-address = localhost
    # 允许最大连接数
    max_connections=200
    # 允许连接失败的次数。这是为了防止有人试图攻击数据库
    max_connect_errors=10
    # 服务端使用的字符集
    character-set-server=utf8mb4
    # 数据库字符集对应一些排序等规则使用的字符集
    collation-server=utf8mb4_general_ci
    # 创建新表时将使用的默认存储引擎
    default-storage-engine=INNODB

    [client]
    port = 3306
    default-character-set=utf8mb4

    [mysql]
    no-auto-rehash
    default-character-set=utf8mb4
    ```

- 第三步：编写MySQL从节点1的配置文件和初始化文件

    ```shell
    # slave1/conf/my.cnf
    [mysqld]
    # 实例id，不能和集群中其他实例相同
    server-id=1
    # bin log 文件的前缀，不需要再往下同步时可不加该项
    # log-bin=mysql-bin
    # 要同步的数据库
    # binlog-do-db=
    # 要忽略的数据库
    binlog-ignore-db=information_schema
    binlog-ignore-db=mysql
    binlog-ignore-db=personalsite
    binlog-ignore-db=test

    # 设置mysql的安装目录
    basedir         = /usr
    # 设置mysql数据库的数据的存放目录
    datadir         = /var/lib/mysql
    tmpdir          = /tmp
    pid-file        = /var/run/mysqld/mysqld.pid
    socket          = /var/run/mysqld/mysqld.sock

    # 设置默认使用的端口，与docker-compose里映射端口保持一致
    port            = 3306
    #一定要注释掉，主节点所在容器与从节点不同
    # bind-address = localhost
    # 允许最大连接数
    max_connections=200
    # 允许连接失败的次数。这是为了防止有人试图攻击数据库
    max_connect_errors=10
    # 服务端使用的字符集
    character-set-server=utf8mb4
    # 数据库字符集对应一些排序等规则使用的字符集
    collation-server=utf8mb4_general_ci
    # 创建新表时将使用的默认存储引擎
    default-storage-engine=INNODB

    [client]
    port = 3306
    default-character-set=utf8mb4

    [mysql]
    no-auto-rehash
    default-character-set=utf8mb4
    ```

- 初始化脚本`init.sql`

    ```sql
    # slave1/init/init.sql
    # 动态配置主节点连接信息
    change master to master_host='master', master_user='root', master_password='123456';
    reset slave;
    # 开启从节点
    start slave;
    ```

> 通过`show slave status;`语句查看主从同步状态

- 第四步：编写MySQL从节点2的配置文件和初始化文件

    ```shell
    # slave2/conf/my.cnf
    [mysqld]
    # 实例id，不能和集群中其他实例相同
    server-id=2
    # bin log 文件的前缀，不需要再往下同步时可不加该项
    # log-bin=mysql-bin
    # 要同步的数据库
    # binlog-do-db=
    # 要忽略的数据库
    binlog-ignore-db=information_schema
    binlog-ignore-db=mysql
    binlog-ignore-db=personalsite
    binlog-ignore-db=test

    # 设置mysql的安装目录
    basedir         = /usr
    # 设置mysql数据库的数据的存放目录
    datadir         = /var/lib/mysql
    tmpdir          = /tmp
    pid-file        = /var/run/mysqld/mysqld.pid
    socket          = /var/run/mysqld/mysqld.sock

    # 设置默认使用的端口，与docker-compose里映射端口保持一致
    port            = 3306
    #一定要注释掉，主节点所在容器与从节点不同
    # bind-address = localhost
    # 允许最大连接数
    max_connections=200
    # 允许连接失败的次数。这是为了防止有人试图攻击数据库
    max_connect_errors=10
    # 服务端使用的字符集
    character-set-server=utf8mb4
    # 数据库字符集对应的字符序
    collation-server=utf8mb4_general_ci
    # 创建新表时将使用的默认存储引擎
    default-storage-engine=INNODB

    [client]
    port = 3306
    default-character-set=utf8mb4

    [mysql]
    no-auto-rehash
    default-character-set=utf8mb4
    ```

- 初始化脚本`init.sql`

    ```sql
    # slave2/init/init.sql
    # 动态配置主节点连接信息
    change master to master_host='master', master_user='root', master_password='123456';
    reset slave;
    # 开启从节点
    start slave;
    ```

> 通过`show slave status;`语句查看主从同步状态

- 第五步：编写Atlas 配置 `docker-atlas.cnf`

    ```shell
    [mysql-proxy]
    # Atlas 的管理用户
    admin-username = snow
    admin-password = 123456
    # 写节点
    # Atlas 后端连接的MySQL主库的IP和端口，可设置多项，用逗号分隔
    proxy-backend-addresses = master:3306
    # 读的节点
    # Atlas 后端连接的MySQL从库的IP和端口，@后面的数字代表权重，用来做负载均衡，若省略则默认为1，可设置多项，用逗号分隔
    proxy-read-only-backend-addresses = slave1:3306@1,slave2:3306@2
    # 用户名与其对应的加密过的MySQL密码，密码使用的PREFIX/bin目录下的加密程序encrypt加密
    pwds = root:/iZxz+0GRoA=
    # 设置 Atlas 的运行方式，设置true时为守护进程方式(后台运行)，false为前台运行，一般开发调试时设为false，线上运行时设为true
    # 后台运行
    daemon = true
    # 检查节点心跳
    # true  : Atlas会启动两个进程，一个为 monitor，一个为 worker，monitor 在 worker意外退出后会重启
    # false : 只有 work，没有 monitor
    # 一般开发调试时设为false，线上运行时设为true
    keepalive = true
    # 并发线程
    event-threads = 8
    # 日志格式
    log-level = message
    # 日志路径
    log-path = /usr/local/mysql-proxy/log
    # SQL日志开关，可设置为 OFF ON REALTIME，OFF代表不记录SQL日志，ON代表记录SQL日志，REALTIME代表记录SQL日志且...，默认为OFF
    sql-log=REALTIME
    # 端口号,多个配置文件多个端口
    proxy-address = 0.0.0.0:1234
    # 管理的端口
    admin-address = 0.0.0.0:2345
    charset=utf8
    ```

    >密码加密，PREFIX/bin目录下的加密程序encrypt加密，就是Atlasl的安装目录:/usr/local/mysql-proxy/bin 执行（例如） ./encrypt ‘123456’  

- 第六步：使用docker-compose 构建镜像并启动容器组服务

  命令如下：

  ```shell
  # 进入docker-compose.yml所在文件夹，输入以下命令构建镜像
  docker-compose build
  # 查看已生成的镜像
  docker images
  # 启动容器组服务(-d 后台运行)
  docker-compose up(start|stop) -d
  # 查看运行中的容器
  docker ps
  ```

### 使用与维护 ###

- 查看主从节点的运行情况  
  通过MySQL命令分别连接主从节点(根据端口映射区分)，或进入容器内进行连接：

    ```shell
    # 进入容器
    docker exec -it container_name /bin/bash
    ```

    ```sql
    # 获得当前mysql实例的server_uuid值（作为识别id）
    mysql> show variables like '%server_uuid%';
    +---------------+--------------------------------------+
    | Variable_name | Value                                |
    +---------------+--------------------------------------+
    | server_uuid   | c152473b-6494-11eb-868d-0242ac120002 |
    +---------------+--------------------------------------+

    # 通过命令 show [master|slave] status，可以查看数据库当前正在使用的日志及当前执行日志位置
    mysql> show master status \G;
    *************************** 1. row ***************************
                File: mysql-bin.000005
            Position: 369
        Binlog_Do_DB:
    Binlog_Ignore_DB: information_schema,mysql,personalsite,test
    Executed_Gtid_Set:
    1 row in set (0.00 sec)


    # show slave status 命令展示结果分析
    Slave_IO_State: Waiting for master to send event
    Master_Host: xxx.xxx.xxx.xxx
    Master_User: root
    Master_Port: 3306
    Connect_Retry: 60
    Master_Log_File: mysql-master-bin.000084//主库的二进制文件
    Read_Master_Log_Pos: 1054617387//主库二进制文件的位置
    Relay_Log_File: db-0001-relay-bin.000011//从库的中继文件
    Relay_Log_Pos: 1054614686//已经读取到从库中继文件的位置
    Relay_Master_Log_File: mysql-master-bin.000084//已经执行到了哪个文件
    Exec_Master_Log_Pos: 1054614459//已经执行到的位置
    Slave_IO_Running: Yes//从服务器正从主服务器上读取BINLOG日志,并写入从服务器的中继日志.
    Slave_SQL_Running: Yes//进程正在读取从服务器的BINLOG中继日志，并转化为SQL执行


    # 查看所有日志列表
    show master logs;
    show binary logs;
    ```

- binlog日志  
  - 开启binlog  
    在配置文件`my.cnf`中[mysqld]区块设置/添加`log-bin=mysql-bin`确认是打开状态(值 mysql-bin 是日志的基本名或前缀名)；
  - 通过mysql的变量配置表，查看二进制日志是否已开启

    ```sql
    mysql> show variables like 'log_%'; 
        +----------------------------------------+---------------------------------------+
        | Variable_name                          | Value                                 |
        +----------------------------------------+---------------------------------------+
        | log_bin                                | ON                                    | ------> ON表示已经开启binlog日志
        | log_bin_basename                       | /usr/local/mysql/data/mysql-bin       |
        | log_bin_index                          | /usr/local/mysql/data/mysql-bin.index |
        | log_bin_trust_function_creators        | OFF                                   |
        | log_bin_use_v1_row_events              | OFF                                   |
        | log_error                              | /usr/local/mysql/data/martin.err      |
        | log_output                             | FILE                                  |
        | log_queries_not_using_indexes          | OFF                                   |
        | log_slave_updates                      | OFF                                   |
        | log_slow_admin_statements              | OFF                                   |
        | log_slow_slave_statements              | OFF                                   |
        | log_throttle_queries_not_using_indexes | 0                                     |
        | log_warnings                           | 1                                     |
        +----------------------------------------+---------------------------------------+
    ```

  - binlog日志内容查看  
    binlog日志有二种查看方式，具体如下：  
    **1.mysql查看binlog**

    ```sql
    show binlog events;   #只查看第一个binlog文件的内容
    show binlog events in 'mysql-bin.000002';#查看指定binlog文件的内容
    show binary logs;  #获取binlog文件列表
    show master status； #查看当前正在写入的binlog文件
    ```

    **2.使用mysqlbinlog工具**
    >mysqlbinlog是一个查看mysql二进制日志的工具，可以把mysql上面的所有操作记录从日志里导出，这个工具默认的安装路径为：/usr/local/mysql/bin/mysqlbinlog  
    >可以通过find / -name "mysqlbinlog"命令查找mysqlbinlog的工具路径。

```shell
# 基于开始/结束时间：
/usr/local/mysql/bin/mysqlbinlog --start-datetime="2013-03-01 00:00:00" --stop-datetime="2014-03-21 23:59:59" /usr/local/mysql/var/mysql-bin.000007 -r  test2.sql
```
