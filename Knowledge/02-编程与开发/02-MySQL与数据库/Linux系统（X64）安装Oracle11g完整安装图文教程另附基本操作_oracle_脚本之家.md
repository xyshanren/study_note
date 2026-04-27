---
url: https://www.jb51.net/article/53769.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: 数据库, DevOps, 设计
content_type: article
word_count: 8000
status: 已处理
---

# Linux系统（X64）安装Oracle11g完整安装图文教程另附基本操作_oracle_脚本之家

> source: [www.jb51.net](https://www.jb51.net/article/53769.htm)

## 核心要点

1. 您的位置：首页 → 数据库 → oracle → Linux安装Oracle11g完整图文教程
2. Linux系统（X64）安装Oracle11g完整安装图文教程另附基本操作
3. 更新时间：2014年08月15日 10:46:06 投稿：hebedich
4. 因项目需求，需要在64位linux系统中安装Oracle 11g，在网上查了很多内容，结合自己的实际经验，终于安装成功，记录下来，分享给有需要的同志们，不谢哈！
5. 1）修改用户的SHELL的限制，修改/etc/security/limits.
## 内容预览

脚本之家

服务器常用软件

手机版

关注微信

快捷导航 

网站首页

网页制作

网络编程

脚本专栏

脚本下载

数据库

服务器

电子书籍

操作系统

网站运营

平面设计

其它

媒体动画

电脑基础

硬件教程

网络安全

MsSql

Mysql

mariadb

oracle

DB2

mssql2008

mssql2005

SQLite

PostgreSQL

MongoDB

Redis

Access

数据库文摘

数据库其它

您的位置：首页 → 数据库 → oracle → Linux安装Oracle11g完整图文教程

Linux系统（X64）安装Oracle11g完整安装图文教程另附基本操作

更新时间：2014年08月15日 10:46:06 投稿：hebedich

因项目需求，需要在64位linux系统中安装Oracle 11g，在网上查了很多内容，结合自己的实际经验，终于安装成功，记录下来，分享给有需要的同志们，不谢哈！^_^

一、修改操作系统核心参数

在Root用户下执行以下步骤：

1）修改用户的SHELL的限制，修改/etc/security/limits.conf文件

输入命令：vi /etc/security/limits.conf，按i键进入编辑模式，将下列内容加入该文件。

oracle soft nproc 2047

oracle hard nproc 16384

oracle soft nofile 1024

oracle hard nofile 65536

编辑完成后按Esc键，输入“:wq”存盘退出

2）修改/etc/pam.d/login 文件，输入命令：vi /etc/pam.d/login，按i键进入编辑模式，将下列内容加入该文件。

session required /lib/security/pam_limits.so

session required pam_limits.so

编辑完成后按Esc键，输入“:wq”存盘退出

3）修改linux内核，修改/etc/sysctl.conf文件，输入命令: vi /etc/sysctl.conf ，按i键进入编辑模式，将下列内容加入该文件

fs.file-max = 6815744

fs.aio-max-nr = 1048576

kernel.shmall = 2097152

kernel.shmmax = 2147483648

kernel.shmmni = 4096

kernel.sem = 250 32000 100 128

net.ipv4.ip_local_port_range = 9000 65500

net.core.rmem_default = 4194304

net.core.rmem_max = 4194304

net.core.wmem_default = 262144

net.core.wmem_max = 1048576

编辑完成后按Esc键，输入“:wq”存盘退出

4）要使 /etc/sysctl.conf 更改立即生效，执行以下命令。 输入：sysctl -p 显示如下：

linux:~ # sysctl -p

net.ipv4.icmp_echo_ignore_broadcasts = 1

net.ipv4.conf.all.rp_filter = 1

fs.file-m...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
