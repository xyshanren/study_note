---
url: http://blog.csdn.net/italyfiori/article/details/43966109
source: blog.csdn.net
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 6000
status: 已处理
---

# PostgreSQL 用户和权限管理_pgsql permission-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/italyfiori/article/details/43966109)

## 核心要点

1. 文章标签：

 #postgresql
 #用户
 #权限

 PostgreSQL
 专栏收录该内容

 2 篇文章

 订阅专栏

 本文介绍了PostgreSQL的用户和权限管理，包括默认用户、登录方法、用户创建、设定用户属性、修改权限、用户组操作以及用户和组的删除。详细讲解了如何通过命令行创建、修改用户，并设置了不同的访问权限
2. 默认用户

postgres安装完成后会自动在操作系统和postgres数据库中分别创建一个名为postgres的用户以及一个同样名为postgres的数据库
3. 登录

方式1:指定参数登录

psql -U username -d database_name -h host -W

 参数含义: -U指定用户 -d要连接的数据库 -h要连接的主机 -W提示输入密码

## 内容预览

PostgreSQL 用户和权限管理

 最新推荐文章于 2026-03-05 00:49:54 发布

 原创

 最新推荐文章于 2026-03-05 00:49:54 发布
 ·
 3.6w 阅读

 ·

 2

 ·

 31

 ·

 CC 4.0 BY-SA版权

 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

 文章标签：

 #postgresql
 #用户
 #权限

 PostgreSQL
 专栏收录该内容

 2 篇文章

 订阅专栏

 本文介绍了PostgreSQL的用户和权限管理，包括默认用户、登录方法、用户创建、设定用户属性、修改权限、用户组操作以及用户和组的删除。详细讲解了如何通过命令行创建、修改用户，并设置了不同的访问权限。

 默认用户

postgres安装完成后会自动在操作系统和postgres数据库中分别创建一个名为postgres的用户以及一个同样名为postgres的数据库。

登录

方式1:指定参数登录

psql -U username -d database_name -h host -W

 参数含义: -U指定用户 -d要连接的数据库 -h要连接的主机 -W提示输入密码。

方式2:切换到postgre...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
