---
url: http://www.cnblogs.com/qixuejia/archive/2010/12/21/1913203.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 847
status: 已处理
---

# MySql 申明变量以及赋值 - Cat Qi - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/qixuejia/archive/2010/12/21/1913203.html)

## 核心要点

1. Imagination is more important than knowledge.
2. 局部变量用一个@标识，全局变量用两个@（常用的全局变量一般都是已经定义好的）；
3. 申明局部变量语法：declare @变量名 数据类型；例如：declare @num int；
4. set @num=value; 或 select @num=value;
5. 如果想获取查询语句中的一个字段值可以用select给变量赋值,如下：
## 内容预览

Cat in dotNET

Imagination is more important than knowledge.

博客园

首页

新随笔

联系

管理

订阅

MySql 申明变量以及赋值

sql server中变量要先申明后赋值：

局部变量用一个@标识，全局变量用两个@（常用的全局变量一般都是已经定义好的）；

申明局部变量语法：declare @变量名 数据类型；例如：declare @num int；

赋值：有两种方法式（@num为变量名，value为值）

set @num=value; 或 select @num=value;

如果想获取查询语句中的一个字段值可以用select给变量赋值,如下：

select @num=字段名 from 表名 where ……

mysql中变量不用事前申明，在用的时候直接用“@变量名”使用就可以了。

第一种用法：set @num=1; 或set @num:=1; //这里要使用变量来保存数据，直接使用@num变量

第二种用法：select @num:=1; 或 select @num:=字段名 from 表名 where ……

注意上面两种赋值符号，使用set时可以用“=”或“：=”，但是使用select时必须用“：=赋值”

作者：Cat Qi

出处：http://qixuejia.cnblogs.com/

本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文连接，否则保留追究法律责任的权利。

posted @
2010-12-21 22:48
Cat Qi
阅读(150561)
评论(8)

收藏
举报

刷新页面返回顶部

公告

博客园
&copy; 2004-2026

浙公网安备 33010602011771号
浙ICP备2021040463号-3

谨以此模板祝贺【博客园开发者征途】系列图书之《你必须知道的.NET》出版发行
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
