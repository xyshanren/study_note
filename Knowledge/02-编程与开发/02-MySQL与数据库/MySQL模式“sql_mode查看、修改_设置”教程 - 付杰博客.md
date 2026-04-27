---
url: https://www.fujieace.com/mysql/sql_mode.html
source: www.fujieace.com
date_saved: 2026-04-23
tags: 数据库, 学习-教程
content_type: article
word_count: 1963
status: 已处理
---

# MySQL模式“sql_mode查看、修改/设置”教程 - 付杰博客

> source: [www.fujieace.com](https://www.fujieace.com/mysql/sql_mode.html)

## 核心要点

1. 站内

必应

 百度

360

搜狗

MySQL服务器可以在不同的SQL模式下运行，并且可以根据sql_mode系统变量的值将这些模式不同地应用于不同的客户端。DBA可以设置全局SQL模式以匹配站点服务器操作要求，并且每个应用程序都可以将其会话SQL模式设置为自己的要求
2.  
模式会影响MySQL支持的SQL语法以及它执行的数据验证检查。这使得在不同环境中使用MySQL以及将MySQL与其他数据库服务器一起使用更加容易
3. 注意：以下查看sql_mode都是最初MySQL5.7默认的SQL模式

## 内容预览

站内

必应

 百度

360

搜狗

MySQL服务器可以在不同的SQL模式下运行，并且可以根据sql_mode系统变量的值将这些模式不同地应用于不同的客户端。DBA可以设置全局SQL模式以匹配站点服务器操作要求，并且每个应用程序都可以将其会话SQL模式设置为自己的要求。
 
模式会影响MySQL支持的SQL语法以及它执行的数据验证检查。这使得在不同环境中使用MySQL以及将MySQL与其他数据库服务器一起使用更加容易。
 
下面是 MySQL5.7.26版本 为例子！
注意：以下查看sql_mode都是最初MySQL5.7默认的SQL模式。
 
一、全局sql_mode
 
1、查看全局sql_mode：
mysql> select @@global.sql_mode;
可以看到，O_ZERO_DATE、NO_ZERO_IN_DATE，我现在想把这两个设置去掉。
只需要按照下面来修改即可！
 
2、修改全局sql_mode：
set @@global.sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'; 
二、当前sql...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
