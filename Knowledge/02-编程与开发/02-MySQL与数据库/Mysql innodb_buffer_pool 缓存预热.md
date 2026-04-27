---
url: https://www.cndba.cn/jane/article/1959#:~:text=1%EF%BC%9A%E9%9C%80%E8%A6%81%E5%9C%A8my.cnf%E4%B8%AD%E5%8A%A0%E5%85%A5%E5%A6%82%E4%B8%8B%E5%91%BD%E4%BB%A4%EF%BC%9A%20innodb_buffer_pool_dump_at_shutdown%3D1%20--%E5%9C%A8%E5%85%B3%E9%97%AD%E6%97%B6%E6%8A%8A%E7%83%AD%E6%95%B0%E6%8D%AEdump%E5%88%B0%E6%9C%AC%E5%9C%B0%E7%A3%81%E7%9B%98,innodb_buffer_pool_load_at_startup%3D1%20--%E5%9C%A8%E5%90%AF%E5%8A%A8%E6%97%B6%E6%8A%8A%E7%83%AD%E6%95%B0%E6%8D%AE%E5%8A%A0%E8%BD%BD%E5%88%B0%E5%86%85%E5%AD%98%202%EF%BC%9Ainnodb_buffer_pool_dump_now%3D1%20--%E9%87%87%E7%94%A8%E6%89%8B%E5%B7%A5%E6%96%B9%E5%BC%8F%E6%8A%8A%E7%83%AD%E6%95%B0%E6%8D%AEdump%E5%88%B0%E6%9C%AC%E5%9C%B0%E7%A3%81%E7%9B%98
source: www.cndba.cn
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 2088
status: 已处理
---

# Mysql innodb_buffer_pool 缓存预热

> source: [www.cndba.cn](https://www.cndba.cn/jane/article/1959#:~:text=1%EF%BC%9A%E9%9C%80%E8%A6%81%E5%9C%A8my.cnf%E4%B8%AD%E5%8A%A0%E5%85%A5%E5%A6%82%E4%B8%8B%E5%91%BD%E4%BB%A4%EF%BC%9A%20innodb_buffer_pool_dump_at_shutdown%3D1%20--%E5%9C%A8%E5%85%B3%E9%97%AD%E6%97%B6%E6%8A%8A%E7%83%AD%E6%95%B0%E6%8D%AEdump%E5%88%B0%E6%9C%AC%E5%9C%B0%E7%A3%81%E7%9B%98,innodb_buffer_pool_load_at_startup%3D1%20--%E5%9C%A8%E5%90%AF%E5%8A%A8%E6%97%B6%E6%8A%8A%E7%83%AD%E6%95%B0%E6%8D%AE%E5%8A%A0%E8%BD%BD%E5%88%B0%E5%86%85%E5%AD%98%202%EF%BC%9Ainnodb_buffer_pool_dump_now%3D1%20--%E9%87%87%E7%94%A8%E6%89%8B%E5%B7%A5%E6%96%B9%E5%BC%8F%E6%8A%8A%E7%83%AD%E6%95%B0%E6%8D%AEdump%E5%88%B0%E6%9C%AC%E5%9C%B0%E7%A3%81%E7%9B%98)

## 核心要点

1. sql语句执行时首先会去缓存里寻找相关数据，如果缓存找不到则再会去磁盘寻找，当我们数据库重新启动之后缓存为空导致所有的操作都是 先经过缓存然后在去磁盘寻找，此时磁盘I/O业务承受不了，因此会带来很大的性能问题
2. 在MySQL启动时或者手动执行加载 时会自动加载热数据大buffer_pool里
3. 只有在正常关闭MySQL服务时才会dump热数据到本次磁盘ib_buffer_pool文件中，异常宕机MySQL则不会dump热数据

## 内容预览

-->

 签到成功

 知道了

 &times;

 -->

 CNDBA社区

 -->

 登录|注册

 签到

-->

 Mysql innodb_buffer_pool 缓存预热

 2017-06-08 10:31

 3240

 2

 原创

 mysql

 作者：

 jane

 本文链接：https://www.cndba.cn/jane/article/1959

 因某些原因重启数据库，然而重启数据库之后导致 buffer中缓存的数据清空。 

 sql语句执行时首先会去缓存里寻找相关数据，如果缓存找不到则再会去磁盘寻找，当我们数据库重新启动之后缓存为空导致所有的操作都是 先经过缓存然后在去磁盘寻找，此时磁盘I/O业务承受不了，因此会带来很大的性能问题。 

 MySQL 5.6版本有 新特性专门用来快速预热 buffer_pool 缓存池 

 1：需要在my.cnf中加入如下命令： 

 innodb_buffer_pool_dump_at_shutdown=1   --在关闭时把热数据dump到本地磁盘 

 innodb_buffer_pool_load_at_startup=1     --在启动时把热数据加载到内存 

2：innodb_buffer_pool_dump_n...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
