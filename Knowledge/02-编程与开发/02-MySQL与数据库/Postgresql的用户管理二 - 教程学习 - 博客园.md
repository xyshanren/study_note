---
url: https://www.cnblogs.com/pxpxstudy/p/3732136.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 数据库, 学习-教程
content_type: article
word_count: 6000
status: 已处理
---

# Postgresql的用户管理二 - 教程学习 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/pxpxstudy/p/3732136.html)

## 核心要点

1. 博客园

首页

新随笔

联系

订阅

管理

 Postgresql的用户管理二

五、给已存在用户赋予各种权限

使用ALTER ROLE 命令
2. ALTER ROLE 语法：

 

ALTER ROLE name [ [ WITH ] option [ ..

## 内容预览

一定要好好努力！好好努力！

博客园

首页

新随笔

联系

订阅

管理

 Postgresql的用户管理二

五、给已存在用户赋予各种权限

使用ALTER ROLE 命令。

ALTER ROLE 语法：

 

ALTER ROLE name [ [ WITH ] option [ ... ] ]

where option can be:

 SUPERUSER | NOSUPERUSER
 | CREATEDB | NOCREATEDB
 | CREATEROLE | NOCREATEROLE
 | CREATEUSER | NOCREATEUSER
 | INHERIT | NOINHERIT
 | LOGIN | NOLOGIN
 | REPLICATION | NOREPLICATION
 | CONNECTION LIMIT connlimit
 | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'
 | VALID UNTIL 'timestamp'

ALTER ROLE name RENAME TO new_name

ALTER ROLE name [ IN DATABASE database_name ] SET configuration_parameter { TO | = } { va...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
