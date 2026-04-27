---
url: https://www.cnblogs.com/yanjieli/p/9807011.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端, 数据库
content_type: article
word_count: 6000
status: 已处理
---

# MySQL逻辑备份mysqldump - 别来无恙- - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/yanjieli/p/9807011.html)

## 核心要点

1. mysqldump+binlog

完全备份（mysqldump）+增量备份（binlog）

适用于中小型数据库；通过结合二进制日志文件，把数据库恢复到

## 内容预览

Loading

YJ.li

-->

管理

 MySQL逻辑备份mysqldump

MySQL 备份之 mysqldump

mysqldump

mysqldump工具备份：

本质：导出的是SQL语句文件

优点：不论是什么存储引擎，都可以用mysqldump备成SQL语句

缺点：速度较慢，导入时可能会出现格式不兼容的突发情况，无法做增量备份和累计增量备份

提供三种级别的备份，表级，库级和全库级

Usage: mysqldump [OPTIONS] database [tables]
OR mysqldump [OPTIONS] --databases [OPTIONS] DB1 [DB2 DB3...]
OR mysqldump [OPTIONS] --all-databases [OPTIONS]

说明：

如果备份对象下的数据库绝大多数都是myisam类型表，为了保证数据的一致性，备份时需要锁定表

如果是针对innodb的表进行备份由于innodb是事务型的引擎，会话与会话之间是隔离的，所以备份的时候不影响数据库的正常使用，无需锁表

--lock-tables 如果备份的数据库里的表与其他库没有关系的话，那么只需要锁定该库下的表就可以了
--lock-all-tables 如果备份的数据库里的表与其他库有关系的话，那么需要锁定整个mysql数据库的所有...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
