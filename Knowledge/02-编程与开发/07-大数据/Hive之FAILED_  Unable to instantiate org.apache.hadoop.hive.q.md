---
url: https://www.cnblogs.com/yszd/p/10626048.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端, 数据库
content_type: article
word_count: 854
status: 已处理
---

# Hive之FAILED:  Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient异常 - 云山之巅 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/yszd/p/10626048.html)

## 核心要点

1. 云山之巅

------自学是你超越他人使自己变的重要的一种能力
2. 二.解决方案

　　方法1.Hive元数据库未初始化

　　　　进入Hive目录Hive/bin，执行命令：schematool -dbType -initSchema mysql

　　　　执行结果：

　　　  　启动Hive

　　　　表示异常已解决
3. 方法2.metastore服务未启动

　　　　执行命令：hive --service metastore & ，接着按Ctrl+C

　　　　结果：　

　　　　 启动Hive　　 　

 　　　　表示异常已解决

## 内容预览

云山之巅

------自学是你超越他人使自己变的重要的一种能力！

博客园

新随笔

联系

-->

管理

 Hive之FAILED: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient异常

一.场景

　　Hive启动不报错，当使用show functions;或create table...时报：FAILED: SemanticException org.apache.hadoop.hive.ql.metadata.HiveException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient异常。

二.解决方案

　　方法1.Hive元数据库未初始化

　　　　进入Hive目录Hive/bin，执行命令：schematool -dbType -initSchema mysql

　　　　执行结果：

　　　  　启动Hive

　　　　表示异常已解决！

　　方法2.metastore服务未启动

　　　　执行命令：hive --service metastore &a...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
