---
url: http://www.linuxidc.com/Linux/2016-05/131749.htm
source: www.linuxidc.com
date_saved: 2026-04-23
tags: Python, 前端, 后端
content_type: article
word_count: 1987
status: 已处理
---

# 查看Linux系统版本信息_Linux教程_Linux公社-Linux系统门户网站

> source: [www.linuxidc.com](http://www.linuxidc.com/Linux/2016-05/131749.htm)

## 核心要点

1. 2、cat /etc/redhat-release，这种方法只适合Redhat系的Linux：

[root@S-CentOS home]# cat /etc/redhat-release
CentOS release 6.5 (Final)

3、cat /etc/issue，此命令也适用于所有的Linux发行版
2. Copyright &copy; 2006-2016　Linux公社　All rights reserved 沪ICP备15008072号-1号

## 内容预览

你好，游客 登录

 注册

 搜索

 首页Linux新闻Linux教程数据库技术Linux编程服务器应用Linux安全Linux下载Linux认证Linux主题Linux壁纸Linux软件数码手机电脑

 首页 → Linux教程

 背景：

 阅读新闻

 查看Linux系统版本信息

 [日期：2016-05-25]

 来源：Linux社区 

 作者：Linux

 [字体：大 中 小]

 一、查看Linux内核版本命令（两种方法）：

1、cat /proc/version

[root@S-CentOS home]# cat /proc/version
Linux version 2.6.32-431.el6.x86_64 (mockbuild@c6b8.bsys.dev.centos.org) (gcc version 4.4.7 20120313 (Red Hat 4.4.7-4) (GCC) ) #1 SMP Fri Nov 22 03:15:09 UTC 2013

2、uname -a

[root@S-CentOS home]# uname -a

Linux S-CentOS 2.6.32-431.el6.x86_64 #1 SMP Fri Nov 22 03:15:09 UTC 2013 x86_64 x86_64 x86_...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
