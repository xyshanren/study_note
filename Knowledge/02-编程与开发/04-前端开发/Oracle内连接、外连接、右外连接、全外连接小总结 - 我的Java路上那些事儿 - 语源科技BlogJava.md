---
url: http://www.blogjava.net/hello-yun/archive/2011/04/08/347890.html
source: www.blogjava.net
date_saved: 2026-04-23
tags: 后端, 设计, 学习-教程
content_type: article
word_count: 5440
status: 已处理
---

# Oracle内连接、外连接、右外连接、全外连接小总结 - 我的Java路上那些事儿 - 语源科技BlogJava

> source: [www.blogjava.net](http://www.blogjava.net/hello-yun/archive/2011/04/08/347890.html)

## 核心要点

1. A．内连接

内连接，即最常见的等值连接，例：

SELECT * 

FROM TESTA,TESTB

WHERE TESTA.A=TESTB.A

结果

 A

 B

 A

 B

 001

 10A

 001

 10B

B.外连接

外连接分为左外连接，右外连接和全外连接

## 内容预览

我的Java路上那些事儿

 快乐成长

posts - 110, comments - 101, trackbacks - 0, articles - 7

 

语源科技BlogJava ::

首页 ::

新随笔 ::

联系 ::

聚合

 ::

管理

 Oracle内连接、外连接、右外连接、全外连接小总结

 Posted on 2011-04-08 14:43 云云 阅读(93250) 评论(12)  编辑  收藏 

-->

数据库版本：Oracle 9i

表TESTA,TESTB,TESTC,各有A, B两列

 A

 B

 001

 10A

 002

 20A

 A

 B

 001

 10B

 003

 30B

 A

 B

 001

 10C

 004

 40C

连接分为两种：内连接与外连接。

A．内连接

内连接，即最常见的等值连接，例：

SELECT * 

FROM TESTA,TESTB

WHERE TESTA.A=TESTB.A

结果

 A

 B

 A

 B

 001

 10A

 001

 10B

B.外连接

外连接分为左外连接，右外连接和全外连接。
...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
