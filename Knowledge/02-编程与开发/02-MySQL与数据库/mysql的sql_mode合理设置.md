---
url: http://xstarcd.github.io/wiki/MySQL/MySQL-sql-mode.html
source: xstarcd.github.io
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 2278
status: 已处理
---

# mysql的sql_mode合理设置

> source: [xstarcd.github.io](http://xstarcd.github.io/wiki/MySQL/MySQL-sql-mode.html)

## 核心要点

1. STRICT_TRANS_TABLES：

在该模式下，如果一个值不能插入到一个事务表中，则中断当前的操作，对非事务表不做限制

NO_ZERO_IN_DATE：

在严格模式下，不允许日期和月份为零

NO_ZERO_DATE：

设置该值，mysql数据库不允许插入零日期，插入零日期会抛出错误而不是警告
2. 如果使用mysql，为了继续保留大家使用oracle的习惯，可以对mysql的sql_mode设置如下：在my.cnf添加如下配置

[mysqld]
sql_mode='ONLY_FULL_GROUP_BY,NO_AUTO_VALUE_ON_ZERO

## 内容预览

首页

 分类首页

mysql的sql_mode合理设置

当前sql-mode设置
sql_mode常用值

http://dev.mysql.com/doc/refman/5.7/en/sql-mode.html

http://blog.csdn.net/wyzxg/article/details/8787878

当前sql-mode设置

查看当前sql-mode

SELECT @@GLOBAL.sql_mode;
SELECT @@SESSION.sql_mode;

mysql> SELECT @@GLOBAL.sql_mode;
+--------------------------------------------+
| @@GLOBAL.sql_mode |
+--------------------------------------------+
| STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION |
+--------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT @@SESSION.sql_mode;
+--------------------------------------------+
...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
