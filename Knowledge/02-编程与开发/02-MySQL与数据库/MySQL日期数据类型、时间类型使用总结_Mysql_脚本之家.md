---
url: http://www.jb51.net/article/23966.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: 数据库, 设计, 学习-教程
content_type: article
word_count: 6000
status: 已处理
---

# MySQL日期数据类型、时间类型使用总结_Mysql_脚本之家

> source: [www.jb51.net](http://www.jb51.net/article/23966.htm)

## 核心要点

1. 另外，timestamp 类型的列还有个特性：默认情况下，在 insert, update 数据时，timestamp 列会自动以当前时间（CURRENT_TIMESTAMP）填充/更新。“自动”的意思就是，你不去管它，MySQL 会替你去处理
2.  

建表的代码为:

create table t8 (
  `id1` timestamp NOT NULL default CURRENT_TIMESTAMP,
  `id2` datetime default NULL
);

一般情况下，我倾向于使用 datetime 日期类型

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

 您的位置：首页 → 数据库 → Mysql → 日期数据 时间类型

 MySQL日期数据类型、时间类型使用总结

  更新时间：2010年06月21日 12:20:08   作者：   

 MySQL日期数据类型、MySQL时间类型使用总结，需要的朋友可以参考下。

 MySQL 日期类型：日期格式、所占存储空间、日期范围 比较。 
日期类型        存储空间       日期格式         &...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
