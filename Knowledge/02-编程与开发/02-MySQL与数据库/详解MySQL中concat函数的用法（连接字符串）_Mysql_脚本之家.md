---
url: http://www.jb51.net/article/100886.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: 数据库, 设计, 学习-教程
content_type: article
word_count: 6000
status: 已处理
---

# 详解MySQL中concat函数的用法（连接字符串）_Mysql_脚本之家

> source: [www.jb51.net](http://www.jb51.net/article/100886.htm)

## 核心要点

1. MySQL中concat函数

使用方法：

CONCAT(str1,str2,…) 

返回结果为连接参数产生的字符串。如有任何一个参数为NULL ，则返回值为 NULL
2. 注意：

如果所有参数均为非二进制字符串，则结果为非二进制字符串
3. 注意：

如果分隔符为 NULL，则结果为 NULL。函数会忽略任何分隔符参数后的 NULL 值

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

 您的位置：首页 → 数据库 → Mysql → MySQL concat函数

 详解MySQL中concat函数的用法（连接字符串）

  更新时间：2016年12月22日 15:12:23   作者：飘渺的悠远   

 本篇文章主要介绍了MySQL中concat函数的用法（连接字符串），在命令行模式下进行测试。具有一定的参考价值，感兴趣的小伙伴们可以参考一下。

 MySQL中concat函数

使用方法：

CONCAT(str1,str2,…) 

返回结果为连接参数产生的字符串。如有任何一个参数为NULL ，则返回值为 NULL。

注意：

如果所有参数均为非二进制字符串，则结果为非二进制字符串。 
...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
