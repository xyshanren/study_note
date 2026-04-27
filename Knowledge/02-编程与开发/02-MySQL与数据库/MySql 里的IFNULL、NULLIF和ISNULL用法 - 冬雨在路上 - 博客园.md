---
url: http://www.cnblogs.com/JuneZhang/archive/2010/08/26/1809306.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 2716
status: 已处理
---

# MySql 里的IFNULL、NULLIF和ISNULL用法 - 冬雨在路上 - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/JuneZhang/archive/2010/08/26/1809306.html)

## 核心要点

1. June's New World

 知识和经验都是一点点积累的！现在努力也不晚，June加油
2. mysql> select isnull(1+1);
-> 0
mysql> select isnull(1/0);
-> 1
使用= 的null 值对比通常是错误的
3. isnull() 函数同 is null比较操作符具有一些相同的特性。请参见有关is null 的说明

## 内容预览

June's New World

 知识和经验都是一点点积累的！现在努力也不晚，June加油！

博客园
  

首页
  

新随笔
  

联系  

管理
  

订阅 

 MySql 里的IFNULL、NULLIF和ISNULL用法

今天用到了MySql里的isnull才发现他和MSSQL里的还是有点区别，现在简单总结一下：

mysql中isnull,ifnull,nullif的用法如下：

isnull(expr) 的用法：
如expr 为null，那么isnull() 的返回值为 1，否则返回值为 0。 
mysql> select isnull(1+1);
-> 0
mysql> select isnull(1/0);
-> 1
使用= 的null 值对比通常是错误的。 

isnull() 函数同 is null比较操作符具有一些相同的特性。请参见有关is null 的说明。

IFNULL(expr1,expr2)的用法：

假如expr1   不为   NULL，则   IFNULL()   的返回值为   expr1; 
...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
