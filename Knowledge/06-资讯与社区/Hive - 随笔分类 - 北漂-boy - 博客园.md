---
url: https://www.cnblogs.com/yjt1993/category/1483839.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: Python
content_type: article
word_count: 5192
status: 已处理
---

# Hive - 随笔分类 - 北漂-boy - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/yjt1993/category/1483839.html)

## 核心要点

1. 那么如何保护这些数据目录没有被清理掉，而是被移动到Hadoop的回收站里面了呢
2. 这里需要使用到hive的如下参数 hiv
 阅读全文

 posted @ 2021-03-31 18:04 
北漂-boy
 阅读(168) 
 评论(0) 
 推荐(0) 

 hive 数据迁移遇到的问题

摘要：一、数据迁移流程 目前没有采用官方给定的命令来进行数据迁移，而是使用创建表+copy数据的方式
3. 整个执行过程消耗十分钟左右，平时任务2-3分钟可以完成，可以明显看到，任务调度出现问题了

## 内容预览

北漂-boy

人生就是一场自己与自己决斗的战场！

博客园

首页

新随笔

联系

订阅

-->

管理

 随笔分类 - 

 Hive

 1
 2

 下一页

 hive 错误记录 之moveing 失败

摘要：1、问题现象 执行insert overwrite 时出现moveing异常 具体异常如下： 2、分析 目前对于分区表执行的是动态分区的插入方式，可以发现其中的分区字段值变成了_HIVE_DEFAULT_PARTITION_，通过查询资料，这是因为在原始表中的分区字段值有null或者值时，在插入
 阅读全文

 posted @ 2021-05-14 11:16 
北漂-boy
 阅读(254) 
 评论(0) 
 推荐(0) 

 hive 如何对重点目录进行保护

摘要：1、防止误删除数据 在hive里面经常执行insert overwrite语句，通常来说，没有执行完的话，默认不会覆盖目标表的数据。但是如果执行完了，目标表的数据就会被清理掉。 那么如何保护这些数据目录没有被清理掉，而是被移动到Hadoop的回收站里面了呢？ 这里需要使用到hive的如下参数 hiv
 阅读全文

 posted @ 2021-03-31 18:04 
北漂-boy
 阅读(168) 
 评论(0) 
 推荐(0) 

 hive...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
