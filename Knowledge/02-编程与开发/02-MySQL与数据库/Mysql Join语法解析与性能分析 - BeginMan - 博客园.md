---
url: http://www.cnblogs.com/BeginMan/p/3754322.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: Python, 数据库, 学习-教程
content_type: article
word_count: 6000
status: 已处理
---

# Mysql Join语法解析与性能分析 - BeginMan - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/BeginMan/p/3754322.html)

## 核心要点

1. BeginMan

Python学习之旅

博客园

首页

新随笔

订阅

-->

管理

 Mysql Join语法解析与性能分析

一．Join语法概述

join 用于多表中字段之间的联系，语法如下：

..
2. FROM table1 INNER|LEFT|RIGHT JOIN table2 ON conditiona

table1:左表；table2:右表
3. JOIN 按照功能大致分为如下三类：

INNER JOIN（内连接,或等值连接）：取得两个表中存在连接匹配关系的记录

## 内容预览

BeginMan

Python学习之旅

博客园

首页

新随笔

订阅

-->

管理

 Mysql Join语法解析与性能分析

一．Join语法概述

join 用于多表中字段之间的联系，语法如下：

... FROM table1 INNER|LEFT|RIGHT JOIN table2 ON conditiona

table1:左表；table2:右表。

JOIN 按照功能大致分为如下三类：

INNER JOIN（内连接,或等值连接）：取得两个表中存在连接匹配关系的记录。

LEFT JOIN（左连接）：取得左表（table1）完全记录，即是右表（table2）并无对应匹配记录。

RIGHT JOIN（右连接）：与 LEFT JOIN 相反，取得右表（table2）完全记录，即是左表（table1）并无匹配对应记录。

注意：mysql不支持Full join,不过可以通过UNION 关键字来合并 LEFT JOIN 与 RIGHT JOIN来模拟FULL join.

接下来给出一个列子用于解释下面几种分类。如下两个表(A,B)

mysql> select A.id,A.name,B.name from A,B where A.id=B.id;
+----+-----------+-------------+
| id | name | name...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
