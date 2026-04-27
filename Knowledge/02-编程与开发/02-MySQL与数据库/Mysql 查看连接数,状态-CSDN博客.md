---
url: http://blog.csdn.net/starnight_cbj/article/details/4492555
source: blog.csdn.net
date_saved: 2026-04-23
tags: 数据库, DevOps, 设计
content_type: article
word_count: 6000
status: 已处理
---

# Mysql 查看连接数,状态-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/starnight_cbj/article/details/4492555)

## 核心要点

1. 命令 show processlist; 
如果是root帐号你能看到所有用户的当前连接。如果是其它普通帐号只能看到自己占用的连接
2. Aborted_connects 尝试已经失败的MySQL服务器的连接的次数
3. Created_tmp_tables 当执行语句时已经被创造了的隐含临时表的数量

## 内容预览

Mysql 查看连接数,状态

 最新推荐文章于 2026-03-02 16:08:13 发布

 转载

 最新推荐文章于 2026-03-02 16:08:13 发布
 ·
 10w+ 阅读

 ·

 21

 ·

 126

 文章标签：

 #mysql
 #variables
 #insert
 #buffer
 #服务器
 #数据库

 MySQL
 专栏收录该内容

 6 篇文章

 订阅专栏

 本文介绍了如何通过调整MySQL配置来增加最大连接数，并展示了如何查看MySQL的状态变量，以监控数据库性能。

 命令 show processlist; 
如果是root帐号你能看到所有用户的当前连接。如果是其它普通帐号只能看到自己占用的连接。 
show processlist;只列出前100条如果想全列出请使用show full processlist; 
mysql> show processlist; 

命令 show status; 

命令show status like %下面变量%;

Aborted_clients 由于客户没有正确关闭连接已经死掉已经放弃的连接数量。 
...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
