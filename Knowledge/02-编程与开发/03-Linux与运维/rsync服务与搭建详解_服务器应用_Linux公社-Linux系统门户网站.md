---
url: https://www.linuxidc.com/Linux/2016-10/136143.htm
source: www.linuxidc.com
date_saved: 2026-04-23
tags: DevOps, 学习-教程
content_type: article
word_count: 4726
status: 已处理
---

# rsync服务与搭建详解_服务器应用_Linux公社-Linux系统门户网站

> source: [www.linuxidc.com](https://www.linuxidc.com/Linux/2016-10/136143.htm)

## 核心要点

1. rsync全称remote sync，是一种更高效、可以本地或远程同步的命令，之所以高效是因为rsync会对需要同步的源和目的进度行对比，只同步有改变的部分，所以比scp命令更高效，但是rsync本身是一种非加密的传输，可以借助-e选项来设置具备加密功能的承载工具进行加密传输
2. 2、远程shell模式，此时可以利用ssh协议承载其数据传输过程
3. 4、服务器模式，此时，rsync可以工作在守护进程，能够接收客户端的数据请求；在使用时，可以在客户端使用rsync命令把文件发送到守护进程，也可以像服务器请求获取文件
4. -c：--checksum，开启校验功能，强行对文件传输进行校验
5. 归档，保留文件的原有属性相当于rlptgoD的选项组合-p:--perms 保留文件的权限
## 内容预览

手机版

你好，游客 登录

注册

首页Linux新闻Linux教程数据库技术Linux编程服务器应用Linux安全Linux下载Linux主题Linux壁纸Linux软件数码手机电脑

首页 → 服务器应用

背景：

阅读新闻

rsync服务与搭建详解

[日期：2016-10-17]

来源：Linux社区 

作者：arkling

[字体：大 中 小]

rsync介绍

rsync全称remote sync，是一种更高效、可以本地或远程同步的命令，之所以高效是因为rsync会对需要同步的源和目的进度行对比，只同步有改变的部分，所以比scp命令更高效，但是rsync本身是一种非加密的传输，可以借助-e选项来设置具备加密功能的承载工具进行加密传输

rsync的工作模式

rsync有四种工作模式分为：

1、shell模式，也称作本地模式

2、远程shell模式，此时可以利用ssh协议承载其数据传输过程

3、列表模式，其工作方式与ls相似，仅列出源的内容：-nv

4、服务器模式，此时，rsync可以工作在守护进程，能够接收客户端的数据请求；在使用时，可以在客户端使用rsync命令把文件发送到守护进程，也可以像服务器请求获取文件

rsync命令选项

-n：测试，在不确定命令是否能按照意愿执行时，务必要实现测试

-v：详细输出模式，--verbose

-q：--quiet，静默模式

-c：--checksum，开启校验功能，强行对文件传输进行校验

-r：--recursive，递归复制

-a: --archives.归档，保留文件的原有属性相当于rlptgoD的选项组合-p:--perms 保留文件的权限

-t: --times 保留文件的时间戳

-l:--links 保留文件的符号链接

-g：--group保留文件的属组

-o：--owner 保留文件的属主

-D：--devices 保留设备文件

-e ssh：表示使用ssh协议作为继承

-z：对文件压缩后传输

--progress：显示进度条

根据同步的方向不同，分为推、拉两种方式，其命令用法为：

此处需要注意的地方有两点：

1、如果使用命令时只指定源而不指定目标，仅会将源以列表的形式显示而不同步

2、rsync命令使用中，如果源参数的末尾有斜线，只会复制指定目录的内容，而不复制目录本身，没有斜线，则会复制目录本身，包括目录

rsync生产环境使用方式

在中小企业的生产环境中经常有这么一种需求，A服务器上的某些重要文件需要每天备份到B服务器上，此时就可以使用rsync+crontab来进行备份，其步骤为：

1、配置rsync服务器端

yum -y install xinetd rsync是位于xinetd守护进程中

2、vim /etc/rsyncd.conf 创建rsync的配置文件

3、配置文件分为全局配置段和模块配置段，注意配置文件内的设置项使用格式

#Global Parameters

pid file = /var/run/rsyncd.pid 设置rsync运行时pid文件的位置

log file = /var/log/rsyncd.log 设置rsync运行时log文件的位置

lock file = /var/run/rsyncd.lock 设置rsync运行时lock文件的位置

uid = nobody

gid = nobody

uid和gid这两个选项的作用是指定在运行rsync时...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
