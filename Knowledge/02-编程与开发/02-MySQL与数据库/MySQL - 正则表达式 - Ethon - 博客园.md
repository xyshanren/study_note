---
url: https://www.cnblogs.com/wakey/p/5867958.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 1935
status: 已处理
---

# MySQL - 正则表达式 - Ethon - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/wakey/p/5867958.html)

## 核心要点

1.  

 ::

首页
 ::

新随笔
 ::

 ::

 ::

管理

 公告

 MySQL - 正则表达式

1
2. Mysql的正则表达式仅仅使SQL语言的一个子集，可以匹配基本的字符、字符串
3. select * from wp_posts where post_name REGEXP 'hello'; 可以检索出列post_name中所有包含hello的行 

2

## 内容预览

Ethon

 为什么要有方法，因为懒惰是一种美德。

 

 ::

首页
 ::

新随笔
 ::

 ::

 ::

管理

 公告

 MySQL - 正则表达式

1. Mysql的正则表达式仅仅使SQL语言的一个子集，可以匹配基本的字符、字符串。 
 select * from wp_posts where post_name REGEXP 'hello'; 可以检索出列post_name中所有包含hello的行 

2. .匹配除\n之外的任意单个字符
 select * from wp_posts where post_name REGEXP '.og';
 .是正则表达式中里一个特殊的字符。它表示匹配一个字符，因此，bog，cog，dog等等都能匹配。

 注意： 
 关于大小写的区分：MySQL中正则表达式匹配（从版本3.23.4后）不区分大小写 。
 如果要区分大小写，应该使用BINARY关键字，如where post_name REGEXP BINARY 'Hello .000'

3. ^匹配字符串开始位置，如查询所有姓王的人名
 select name from 表名 where name REGEXP '^王';

4. $匹配字符串结束位置，如查询所有姓名末尾是“明”的人名

 selec...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
