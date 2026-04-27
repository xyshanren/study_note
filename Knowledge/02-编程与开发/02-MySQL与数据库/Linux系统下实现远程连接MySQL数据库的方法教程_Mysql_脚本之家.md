---
url: http://www.jb51.net/article/115341.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: 数据库, DevOps, 设计
content_type: article
word_count: 3427
status: 已处理
---

# Linux系统下实现远程连接MySQL数据库的方法教程_Mysql_脚本之家

> source: [www.jb51.net](http://www.jb51.net/article/115341.htm)

## 核心要点

1. root是第1点设置的用户名，密码也是第1点设置的密码

一些细节

在网上找了很多文章，说要开启3306端口才能连接，但是我开启了却还是无法连接，后来又找到了一些文章，说要更改my.cnf，也就是上面的第2点，更改了然后重启服务器就可以了
2. 刚刚在另外一台服务器上面试了一下，没有配置过端口，通过上面三步，很快就连上了
3. 所以第二点非常重要，基本上每个人装mysql的时候都会去配置那个文件，因为字符集需要配置。所以肯定有那个文件的，用find命令找找就行了

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

 您的位置：首页 → 数据库 → Mysql → Linux远程连接MySQL数据库

 Linux系统下实现远程连接MySQL数据库的方法教程

  更新时间：2017年06月04日 14:53:01   作者：CSU_IceLee   

 MySQL默认root用户只能本地访问，不能远程连接管理mysql数据库，Linux如何开启mysql远程连接？下面这篇文章主要给大家介绍了在Linux系统下实现远程连接MySQL数据库的方法教程，需要的朋友可以参考借鉴，下面来一起看看吧。

 前言

最近在工作中遇到了这个需求，估计搞了一个多小时才把这个远程连接搞好。一台本地电脑，一台云服务器，都是linux系统。下面来看看详细的介绍...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
