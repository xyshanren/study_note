---
url: http://blog.csdn.net/zkf11387/article/details/7633492
source: blog.csdn.net
date_saved: 2026-04-23
tags: 后端
content_type: article
word_count: 6000
status: 已处理
---

# oracle什么时候需要commit_alter table需要commit吗-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/zkf11387/article/details/7633492)

## 核心要点

1. DDL  

Data Definition Language (DDL) statements are used to define the database structure or schema

## 内容预览

oracle什么时候需要commit

 最新推荐文章于 2023-03-09 20:56:54 发布

 转载

 最新推荐文章于 2023-03-09 20:56:54 发布
 ·
 5.6w 阅读

 ·

 22

 ·

 54

 文章标签：

 #oracle
 #数据库
 #insert
 #table
 #structure
 #delete

今天在oracle的SQL plus 中执行了删除和查询操作然后在PL/SQL中也执行查询操作语句一样结果却不一样让我大感郁闷后来才突然想到可能是两边数据不一致造成的但是为什么不一致呢就是没用commit

在网上查了一下大概是这样说的

DML语言比如updatedeleteinsert等修改表中数据的需要commit;

DDL语言比如createdrop等改变表结构的就不需要写commit因为内部隐藏了commit;

DDL 数据定义语言

create table 创建表  ...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
