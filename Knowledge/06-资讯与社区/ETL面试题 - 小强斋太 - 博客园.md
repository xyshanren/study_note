---
url: http://www.cnblogs.com/xqzt/p/4472111.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 设计
content_type: article
word_count: 6000
status: 已处理
---

# ETL面试题 - 小强斋太 - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/xqzt/p/4472111.html)

## 核心要点

1. 小强斋太-Study Notes

 ETL面试题

 ETL面试题

1
2. What is a logical data mapping and what does it mean to the ETL team
3. 答：逻辑数据映射（Logical Data Map）用来描述源系统的数据定义、目标数据仓库的模型以及将源系统的数据转换到数据仓库中需要做操作和处理方式的说明文档，通常以表格或Excel的格式保存如下的信息：
 目标表名：
 目标列名：
 目标表类型：注明是事实表、维度表或支架维度表

## 内容预览

小强斋太-Study Notes

 ETL面试题

 ETL面试题

1. What is a logical data mapping and what does it mean to the ETL team?
 什么是逻辑数据映射？它对ETL项目组的作用是什么？
 答：逻辑数据映射（Logical Data Map）用来描述源系统的数据定义、目标数据仓库的模型以及将源系统的数据转换到数据仓库中需要做操作和处理方式的说明文档，通常以表格或Excel的格式保存如下的信息：
 目标表名：
 目标列名：
 目标表类型：注明是事实表、维度表或支架维度表。
 SCD类型：对于维度表而言。
 源数据库名：源数据库的实例名，或者连接字符串。
 源表名：
 源列名：
 转换方法：需要对源数据做的操作，如Sum(amount)等。
 逻辑数据映射应该贯穿数据迁移项目的始终，在其中说明了数据迁移中的ETL策略。在进行物理数据映射前进行逻辑数据映射对ETL项目组是重要的，它起着元数据的作用。项目中最好选择能生成逻辑数据映射的数据迁移工具。

 2. What are the primary goals of the data discovery phase of the data warehouse project?
 在数据仓库项目中，数据探索阶段的主要目的是什么？
 答：在逻辑数据映射进行之前...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
