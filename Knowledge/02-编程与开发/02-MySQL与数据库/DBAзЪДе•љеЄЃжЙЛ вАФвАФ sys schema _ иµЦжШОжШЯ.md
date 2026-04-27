---
url: http://mingxinglai.com/cn/2016/03/sys-schema/
source: mingxinglai.com
date_saved: 2026-04-23
tags: 数据库, DevOps, 设计
content_type: article
word_count: 8000
status: 已处理
---

# DBAзЪДе•љеЄЃжЙЛ вАФвАФ sys schema | иµЦжШОжШЯ

> source: [mingxinglai.com](http://mingxinglai.com/cn/2016/03/sys-schema/)

## 核心要点

1. 首先，本文概要地介绍了sys schema的作用和定位；其次，分别介绍了sys schema中的视图、函数和存储过程；接下来，通过两个例子来演示sys schema的用法，便于大家理解sys schema带来的实实在在的好处；最后讨论了sys schema还可以增加的内容。
2. 该项目专注于MySQL的易用性，例如，我们可以通过sys schema快速的知道，哪些语句使用了临时表，哪个用户请求了最多的io，哪个线程占用了最多的内存，哪些索引是无用索引等。
3. 引入sys schema以后，MySQL的易用性将会得到极大地提升，MySQL的用户分析问题和定位问题，将更多的依赖sys schema，减少外部工具的使用。
4. 前面说过，sys schema中包含了大量的视图（只有sys_config是innodb表），那么，这些视图的信息来自哪里呢？
5. 视图中的信息均来自performance schema和information schema中的统计信息。
## 内容预览

本文详细地介绍了MySQL 5.7新引入的sys schema。首先，本文概要地介绍了sys schema的作用和定位；其次，分别介绍了sys schema中的视图、函数和存储过程；接下来，通过两个例子来演示sys schema的用法，便于大家理解sys schema带来的实实在在的好处；最后讨论了sys schema还可以增加的内容。

1. sys schema的介绍

sys schema是MySQL 5.7.7中引入的一个系统库，包含了一系列视图、函数和存储过程，
该项目专注于MySQL的易用性，例如，我们可以通过sys schema快速的知道，哪些语句使用了临时表，哪个用户请求了最多的io，哪个线程占用了最多的内存，哪些索引是无用索引等。

引入sys schema以后，MySQL的易用性将会得到极大地提升，MySQL的用户分析问题和定位问题，将更多的依赖sys schema，减少外部工具的使用。

前面说过，sys schema中包含了大量的视图（只有sys_config是innodb表），那么，这些视图的信息来自哪里呢？视图中的信息均来自performance schema和information schema中的统计信息。MySQL Server blog中有一个很好的比喻：

For Linux users I like to compare performance_schema to /proc, and SYS to vmstat.

也就是说，performance schema和information schema中提供了信息源，但是，没有很好的将这些信息组织成有用的信息，从而没有很好的发挥它们的作用。而sys schema使用performance schema和information schema中的信息，通过视图的方式给出解决实际问题的答案。这就是sys schema的作用和目的，也是为什么sys schema值得我们花点时间学习的原因。

2. sys schema中的视图、函数和存储过程

可以通过以下语句快速查看sys schema包含的视图、函数和存储过程

show full tables from sys
show function status where db = 'sys';
show procedure status where db = 'sys'

接下来将依次给出所有的视图、函数和存储过程，并进行简单的分析，希望能够达到抛砖引玉的效果。

2.1 视图

sys schema中的视图（和一张表）如下，通过名称就很容易猜到具体是做什么用的。

mysql> select table_name , table_type, engine from information_schema.tables where table_schema = 'sys' order by table_name;
+-----------------------------------------------+------------+--------+
| table_name | table_type | engine |
+-----------------------------------------------+------------+--------+
| host_summary | VIEW | NULL |
| host_summary_by_file_io | VIEW | NULL |
| ...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
