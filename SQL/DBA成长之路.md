<!--
 * @Autor: 李逍遥
 * @Date: 2021-02-05 17:23:39
 * @LastEditors: 李逍遥
 * @LastEditTime: 2021-02-13 08:37:19
 * @Descriptiong: DBA的学习指南
-->

# DBA的成长之路 #

- [DBA的成长之路](#dba的成长之路)
  - [概念](#概念)
    - [数据库产品](#数据库产品)
    - [MySQL企业版GA选择](#mysql企业版ga选择)
  - [需要学习的内容](#需要学习的内容)
    - [1.MySQL 5.7 安装部署（二进制），编译自己的扩展](#1mysql-57-安装部署二进制编译自己的扩展)
    - [2.MySQL升级步骤扩展](#2mysql升级步骤扩展)
    - [3.MySQL 5.7 体系结构](#3mysql-57-体系结构)
    - [4.MySQL基础管理](#4mysql基础管理)
    - [5.基础SQL语句使用](#5基础sql语句使用)
    - [6.SQL高级应用](#6sql高级应用)
    - [7.Information_schema获取元数据](#7information_schema获取元数据)
    - [8.索引、执行计划管理（基础优化）](#8索引执行计划管理基础优化)
    - [9.存储引擎](#9存储引擎)
    - [10.日志管理](#10日志管理)
    - [11.备份与恢复](#11备份与恢复)
    - [12.主从复制及架构演变](#12主从复制及架构演变)
    - [13.传统的高可用及读写分离（MHA&Atlas）](#13传统的高可用及读写分离mhaatlas)
    - [14.传统分布式架构设计与实现-扩展（Mycat-->DBLE,DRDS）](#14传统分布式架构设计与实现-扩展mycat--dbledrds)
    - [15.MySQL 5.7 高可用及分布式架构-扩展（MGR,InnoDB Cluster）](#15mysql-57-高可用及分布式架构-扩展mgrinnodb-cluster)
    - [16.MySQL优化（安全，性能）](#16mysql优化安全性能)
    - [17.MySQL监控（zabbix,open-falcon）](#17mysql监控zabbixopen-falcon)
    - [18.RDS（阿里云）](#18rds阿里云)
  - [还需要学习的Nosql](#还需要学习的nosql)
    - [1.Redis](#1redis)
    - [2.MongoDB](#2mongodb)
  - [另外需要了解的关系型数据库](#另外需要了解的关系型数据库)
    - [1.Oracle](#1oracle)
    - [2.postgresql](#2postgresql)

## 概念 ##

  DBA —— 数据库管理员

>数据库的发展情况大概如下（目前还处在第二个阶段）：
>RDBMS(关系型数据库) --> NOSQL+RDBMS --> NOSQL(RDBMS),RDBMS(NOSQL) --> NewSQL

### 数据库产品 ###

- **RDBMS**
  Oracle,MySQL,PostgreSQL,SqlServer

- **NoSQL**
  MongoDB,Redis,Elasticsearch

- **NewSQL(特点是分布式)**
  TiDB,Spanner,PolarDB

### MySQL企业版GA选择 ###

- **大版本（主流）：5.6,5.7,8.0**
  各大版本中的主流版本  
  5.6 : 5.6.34 , 5.6.36 , 5.6.38(2017913) , 5.6.40  
  5.7 : 5.7.18 , 5.7.20(2017913) , 5.7.24
  8.0 : 8.0.14 , 8.0.15 , 8.0.16

## 需要学习的内容 ##

### 1.MySQL 5.7 安装部署（二进制），编译自己的扩展 ###

- **手动安装部署，以Linux通用版(generic)为例**

  - 下载并上传二进制文件或者直接使用 wget 命令下载，解压并移动到指定目录
    这里使用 `mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz` 版本，命令如下：

    ```shell
    # 解压
    tar xf mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz
    # 创建目录
    mkdir /application
    # 将MySQL文件移动并重命名
    mv mysql-5.7.26-linux-glibc2.12-x86_64 /application/mysql
    ```

  - 处理原始环境

    ```shell
    # 查看是否安装 mariadb
    rpm -qa|grep mariadb
    # 如果有，使用下面的命令卸载掉相关软件，否则会初始化失败
    rpm -e mariadb
    # 如果因为被依赖而无法卸载的话，可以使用以下命令进行卸载
    yum remove mariadb-libs-xxxx -y
    # 也可以加上 --nodeps 不检查依赖关系强制卸载
    rpm -e --nodeps mariadb
    ```

  - 配置环境变量

    ```shell
    # 配置环境变量
    vim /etc/profile
    # 在最后一行添加下面的代码
    export PATH=/application/mysql/bin:$PATH
    # 让配置生效
    source /etc/profile
    # 查看MySQL版本(确认环境变量是否生效)
    mysql -V
    ```

  - 挂载数据盘

    ```shell
    # 创建数据路径
    # 在虚拟机上可以添加一块新磁盘模拟数据盘
    # 查看磁盘情况
    fdisk -l
    # 格式化
    mkfs.xfs /dev/sdb
    # 创建目录
    mkdir /data
    # 挂载
    # 查看磁盘的UUID
    blkid
    # 在配置文件中将磁盘挂载到 data 目录下
    vim /etc/fstab
    # 添加以下代码
    UUID=xxxxxx-xxxx-xxxx-xxxx-xxxxxx /data xfs defaults 0 0
    # 自动挂载
    mount -a
    # 查看是否挂载成功
    df -h
    # 未挂载成功的话，也可以使用以下方法
    cd /sys/class/scsi_host
    echo "---" > host0/scan # 接口扫描新加磁盘
    ```

  - 创建用户并授权

    ```shell
    # 创建管理MySQL的用户（不需要有登录权限）
    useradd -s /sbin/nologin mysql
    # 授权
    mkdir /data/mysql/data -p
    chown -R mysql.mysql /application/*
    chown -R mysql.mysql /data
    ```

  - 初始化数据（创建系统数据）

    ```shell
    # 5.6的命令是：/application/mysql/scripts/mysql_install_db
    /application/mysql/mysqld --initialize --user=mysql --basedir=/application/mysql --datadir=/data/mysql/data
    # 如果初始化报错缺少 libaio 的话，需安装 libaio-devel
    yum install -y libaio-devel
    # 初始化后会生成一个临时密码，如下：
    #### ..... A temporary password is generated for root@localhost: xxxxxx
    ```

    >参数说明：
    >initialize
    >1.对密码复杂度进行定制，包含四种字符且达到12位；
    >2.给root@localhost 用户设置临时密码；
    >initialize-insecure : 无限制无临时密码，生产中往往使用该方式初始化。

  - 配置文件

    ```shell
    # 准备配置文件
    # 在 /etc/my.cnf 中写入以下项(最基本的配置项)
    cat >/etc/my.cnf <<EOF
    [mysqld]
    user=mysql
    basedir=/application/mysql
    datadir=/data/mysql/data
    socket=/tmp/mysql.sock
    server_id=6
    port=3306
    [mysql]
    socket=/tmp/mysql.sock
    EOF
    ```

  - 启动数据库
    1.使用 sys-v

    ```shell
    # 进入命令所在目录
    cd /application/mysql/support-files/
    # 启动
    ./mysql.server start

    # 还可以将 mysql.server 命令放到init.d中管理
    ## 将命令拷贝到 init.d
    cp /application/mysql/support-files/mysql.server /etc/init.d/mysqld
    ## 启动
    service mysqld start

    # 以上还包括 start|stop|restart|status 等
    ```

    2.使用systemd管理MySQL服务（5.7的新特性）

    ```shell
    # 将MySQL服务加入systemd
    cat >/etc/systemd/system/mysqld.service <<EOF
    [Unit]
    Description=MySQL Server
    Documentation=man:mysqld(7)
    Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
    After=network.target
    After=syslog.target
    [Install]
    WantedBy=multi-user.target
    [Service]
    User=mysql
    Group=mysql
    ExecStart=/application/mysql/bin/mysqld --defaults-file=/etc/my.cnf
    LimitNOFILE = 5000
    EOF

    # 启动
    systemctl start mysqld
    ## 判断服务是否启动
    netstat -lnp|grep mysqld
    netstat -lnp|grep 3306
    ps -ef |grep mysqld
    systemctl status mysqld
    ss -tulpn|grep mysqld
    ss -tulpn|grep 3306
    lsof -i :3306
    ```

- **分析处理MySQL数据库无法启动的问题**
  例如：类似 `without updating PID file` 的错误；
  如果控制台上的报错信息没有其他具体信息的话，需要查看日志，日志位置在 `/data/mysql/data/主机名.err`,找 [error] 项的上下文进行分析；
  **可能的原因：**
  a.配置文件 `/etc/my.cnf` 路径不对；
  b. `/tmp/mysql.sock` 文件被修改或者删除；
  c.数据目录未给mysql用户权限；
  d.配置文件 `/etc/my.cnf` 参数错误；

- **管理员密码管理（root@localhost）**
  - 设置/修改密码

    ```shell
    # 该命令需要输入旧密码，有则输入无则跳过
    mysqladmin -root -p password newpassword
    ```

  - 找回管理员密码
    1.先关闭数据库

    ```shel
    systemctl stop mysqld
    ```

    2.使用维护模式启动数据库

    ```shell
    mysqld_safe --skip-grant-tables --skip-networking &
    ```

    3.登录并修改密码

    ```shell
    # 登录MySQL
    mysql
    # 修改密码
    alter user root@'localhost' indentified by 'pwd';
    # 上面的命令会失败，刷新后再修改就成功了
    flush privileges;
    ```

    4.关闭数据库，正常启动，就可以使用新密码登录了

    ```shell
    kill mysqld
    systemctl start mysqld
    ```

### 2.MySQL升级步骤扩展 ###

### 3.MySQL 5.7 体系结构 ###

- **MySQL C/S结构介绍**

  ![m1](mysql-cs.webp "MySQL C/S模型")

  两种连接方法：网络连接串和套接字文件

  ```shell
  #TCP/IP方式（远程、本地）：
  mysql -uroot -p -h xx.x.x.xx -P3306
  #Socket方式(仅本地)：
  mysql -uroot -p -S /tmp/mysql.sock # -S可省略
  ```

- **MySQL实例的构成**
  实例：mysqld守护进程 + master thread + task thread + 预分配内存。

- **MySQL中myslqd服务的结构**

  ![m2](mysql-mysqld.webp "mysqld服务的结构")

  - **连接层**
    - 1.提供连接协议：Socket和TCP/IP
    - 2.提供验证：用户、密码，IP，SOCKET
    - 3.派生一个专用的线程（接收SQL，返回结果），使用`show processlist;` 命令可查看连接的线程

    >思考：
    >使用维护模式找回密码时，启动命令使用了以下参数分别是什么意思？
    >--skip-grant-tables : 不启动用户密码验证
    >--skip-networking : 不启用TCP/IP连接

  - **SQL层（在SQL优化方面至关重要）**
    - 1.接收上层传递的SQL语句
    - 2.语法验证模块：验证语句语法，是否满足SQL_MODE
    - 3.语义检查：判断语句的类型（DDL,DML等）
    - 4.权限检查：用户对库表的权限
    - 5.解析器：对语句执行前,进行预处理，生成解析树(执行计划)，即生成多种执行方案
    - 6.优化器：根据解析器得出的多种执行计划，进行判断，选择最优的执行计划（代价最低的，5.7以后是代价模型）
        代价模型：资源（CPU  IO  MEM）的损耗评估性能好坏。
    - 7.执行器：根据最有执行计划执行SQL语句，产生结果（在磁盘的哪个位置上）
    - 8.提供查询缓存（默认是没开启的），会使用redis tair替代查询缓存功能
    - 9.提供日志记录（具体见日志管理章节）：binlog，默认是没开启的

  - **存储引擎层**
    真正和磁盘打交道的层次，类似Linux中的文件系统。
    根据SQL层提供的地址从磁盘上拿到数据，返回给SQL，结构化成表，再通过连接层返回给用户。

- **逻辑结构**

  ![m3](mysql-jg.webp "逻辑结构")

- **物理结构**

  ![m4](mysql-physics.webp "物理结构")

  *面试题：*
  说明MyISAM和InnoDB在存储方式上的异同？

  - **微观结构**

    页（page）：最小的存储（IO）单元，默认16k
    区：64个（默认）连续的页，共1M
    段：一个表就是一个段，包含一个或多个区
    总结：一个表就是一个段，MySQL分配空间时至少分配一个区，每个区默认是1M（64个页），MySQL最小的IO单元是page（16K）。

- **MySQL启动和关闭**
- **MySQL初始化配置**
- **MySQL多实例**

### 4.MySQL基础管理 ###

- **用户管理**
  - **作用**
    登录数据库
    管理数据库对象

  - **定义**
    用户名@'白名单'。
    白名单：IP地址。
    例如：
    root@'10.0.0.51'
    root@'10.0.0.%'
    root@'10.0.0.5%'
    root@'10.0.0.0/255.255.254.0'
    root@'%'
    root@'xxx.com'
    root@'localhost'
    root@'db01'

    最常用的是以下四种：
    root@'10.0.0.%'
    root@'10.0.0.5%'
    root@'10.0.0.0/255.255.254.0'
    root@'localhost'

  - **操作**
    创建用户

    ```sql
    create user lee@'10.0.0.%' identified by 'pwd';
    ```

    查询用户

    ```sql
    # 查看user表的结构
    desc mysql.user;
    # 查看user表
    select user,host,authentication_string from mysql.user;
    ```

### 5.基础SQL语句使用 ###

结构化查询语言(Structured Query Language)，主要分为4大类：数据查询语言（DQL:Data Query Language）、数据操作语言（DML:Data Manipulation Language）、数据控制语言（DCL:Data Control Language）、数据定义语言（DDL:Data Definition Language）。
>多数时候 DQL也会归为DML。

### 6.SQL高级应用 ###

### 7.Information_schema获取元数据 ###

### 8.索引、执行计划管理（基础优化） ###

### 9.存储引擎 ###

### 10.日志管理 ###

### 11.备份与恢复 ###

### 12.主从复制及架构演变 ###

### 13.传统的高可用及读写分离（MHA&Atlas） ###

### 14.传统分布式架构设计与实现-扩展（Mycat-->DBLE,DRDS） ###

### 15.MySQL 5.7 高可用及分布式架构-扩展（MGR,InnoDB Cluster） ###

### 16.MySQL优化（安全，性能） ###

### 17.MySQL监控（zabbix,open-falcon） ###

### 18.RDS（阿里云） ###

## 还需要学习的Nosql ##

### 1.Redis ###

### 2.MongoDB ###

## 另外需要了解的关系型数据库 ##

### 1.Oracle ###

### 2.postgresql ###



