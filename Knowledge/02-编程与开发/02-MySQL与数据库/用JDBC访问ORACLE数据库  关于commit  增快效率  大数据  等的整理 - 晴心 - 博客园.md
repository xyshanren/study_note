---
url: http://www.cnblogs.com/qingxinblog/p/3375147.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端, 数据库, 效率工具
content_type: article
word_count: 3022
status: 已处理
---

# 用JDBC访问ORACLE数据库  关于commit  增快效率  大数据  等的整理 - 晴心 - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/qingxinblog/p/3375147.html)

## 核心要点

1. 晴心

博客园

首页

新随笔

联系

管理

订阅

 用JDBC访问ORACLE数据库 关于commit 增快效率 大数据 等的整理

1、问：用JDBC访问ORACLE数据库，做DELETE操作，能用JAVA多线程实现吗
2. ORACLE服务器要怎么配？（以下答案来自网络，仅供参考）

     答： Oracle有自己的锁机制。就算你开100条线，它还是一条一条删除。不能同时删除多项的
3.      对于大量数据更新，Oracle有建议一些优化措施

## 内容预览

晴心

博客园

首页

新随笔

联系

管理

订阅

 用JDBC访问ORACLE数据库 关于commit 增快效率 大数据 等的整理

1、问：用JDBC访问ORACLE数据库，做DELETE操作，能用JAVA多线程实现吗？ ORACLE服务器要怎么配？（以下答案来自网络，仅供参考）

     答： Oracle有自己的锁机制。就算你开100条线，它还是一条一条删除。不能同时删除多项的。
     对于大量数据更新，Oracle有建议一些优化措施。
     (1) 首先是把auto-commit给关闭。因为你每删一条数据，oracle就要自动执行一次commit。commit是需要资源的。所以如果你手动设置为每删数据1000条，执行一次commit. 那你就可以节省资源了。
     (2) 充分利用batch update。如果不用batch，每个delete命令都需要从网络上传送到oracle。1万个删除命令，要有1万次传送。如果将100个删除命令绑在一起送去Oracle执行。那就只要传送100次就可以了。大大缩短所需时间和网络资源。
     以上这些建议，都可以在Oracle参考里查到。

2、...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
