---
url: https://www.jb51.net/article/104129.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: Python, DevOps, 设计
content_type: article
word_count: 3889
status: 已处理
---

# linux系统下的ssh登录和配置方法_unix linux_脚本之家

> source: [www.jb51.net](https://www.jb51.net/article/104129.htm)

## 核心要点

1. 更新时间：2017年01月24日 09:29:04 作者：buzhbuzh
2. [root@westos Desktop]# ssh root@192.
3. 26 maps to bogon, but this does not map back to the address - POSSIBLE BREAK-IN ATTEMPT!
4. Last login: Tue Jan 17 13:27:29 2017 from 192.
5. 在服务器的/etc/ssh/sshd_cinfig文件下可以管理ssh服务：
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

安装教程

Windows2003

WindowsXP

注册表

unix linux

其它相关

Vista

密码恢复攻略

系统维护

windows2008

您的位置：首页 → 操作系统 → unix linux → linux 下ssh登录

linux系统下的ssh登录和配置方法

更新时间：2017年01月24日 09:29:04 作者：buzhbuzh

这篇文章给大家介绍了linux系统下的ssh登录方法和ssh的配置方法，ssh有两种登录方式，具体哪两种方式大家通过本文学习

一 ssh的两种登录方式

1密码登录：

[root@westos Desktop]# ssh root@192.168.122.26 

Address 192.168.122.26 maps to bogon, but this does not map back to the address - POSSIBLE BREAK-IN ATTEMPT! 

root@192.168.122.26's password: 

Last login: Tue Jan 17 13:27:29 2017 from 192.168.122.1 

2公钥密钥登录

客户端连接服务器时候会生成.ssh目录,此时保证客户端下生成.ssh目录

第一步服务器端生成公钥和密钥

查看.ssh文件下的内容；

第二步：服务器添加认证

第四步：服务器分发私钥给客户端

完成之后就可以让指定IP的客户端不用密码连接

二ssh的配置

在服务器的/etc/ssh/sshd_cinfig文件下可以管理ssh服务：

PasswordAuthentication yes/on ----------------------> 开启或者关闭密码连接

PermitRootLogin yes/no ----------------------------->允许超级用户登录

AllowUsers student----------------------------->只允许登录的用户

DenyUsers student-------------------------->不允许登录的用户

好了，下面介绍下Linux ssh登录命令

ssh命令用于远程登录上Linux主机。

常用格式：ssh [-l login_name] [-p port] [user@]hostname

更详细的可以用ssh -h查看。

举例

不指定用户：

ssh 192.168.0.11

指定用户：

ssh -l root 192.168.0.11

ssh root@192.168.0.11

如果修改过ssh登录端口的可以：

ssh -p 12333 192.168.0.11

ssh -l root -p 12333 216.230.230.114

ssh -p 12333 root@216.230.230.114

另外修改配置文件/etc/ssh/sshd_config，可以改ssh登录端口和禁止root登录。改端口可以防止被端口扫描。

编辑配置文件：

vim /etc/ssh/sshd_confi...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
