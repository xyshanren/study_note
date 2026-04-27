---
url: https://www.cnblogs.com/qingyunzong/category/1191578.html?page=1
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 学习-教程, 效率工具
content_type: article
word_count: 5138
status: 已处理
---

# Hive - 随笔分类 - 扎心了，老铁 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/qingyunzong/category/1191578.html?page=1)

## 核心要点

1. 摘要：一、Hadoop 框架计算特性 1、数据量大不是问题，数据倾斜是个问题 2、jobs 数比较多的作业运行效率相对比较低，比如即使有几百行的表，如果多次关联多次 汇总，产生十几个 jobs，耗时很长。
2. 原因是 map reduce 作业初始化的时间是比较长的 3、sum,count,max,min 等
3. 摘要：一、Hive 执行过程概述 1、概述 （1） Hive 将 HQL 转换成一组操作符（Operator），比如 GroupByOperator, JoinOperator 等 （2）操作符 Operator 是 Hive 的最小处理单元 （3）每个操作符代表一个 HDFS 操作或者 MapReduc
4. 由于数据分布不均匀，造成数据大量的集中到一点，造成数据热点 2、Hadoop 框架的特性 A、不怕数据大，怕数据倾斜 B、Jobs 数比较多的作业运行效率相对比较低，如子查询比较多 C、 sum,count,max,min 等聚集函数，通常不会有数据倾斜问题 3、主要表现 任务
5. 摘要：一、Hive的命令行 1、Hive支持的一些命令 Command Description quit Use quit or exit to leave the interactive shell.
## 内容预览

扎心了，老铁

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

Hive学习之路 （二十一）Hive 优化策略

摘要：一、Hadoop 框架计算特性 1、数据量大不是问题，数据倾斜是个问题 2、jobs 数比较多的作业运行效率相对比较低，比如即使有几百行的表，如果多次关联多次 汇总，产生十几个 jobs，耗时很长。原因是 map reduce 作业初始化的时间是比较长的 3、sum,count,max,min 等
阅读全文

posted @ 2018-04-15 15:46
扎心了，老铁
阅读(20126)
评论(3)
推荐(12)

Hive学习之路 （二十）Hive 执行过程实例分析

摘要：一、Hive 执行过程概述 1、概述 （1） Hive 将 HQL 转换成一组操作符（Operator），比如 GroupByOperator, JoinOperator 等 （2）操作符 Operator 是 Hive 的最小处理单元 （3）每个操作符代表一个 HDFS 操作或者 MapReduc
阅读全文

posted @ 2018-04-15 15:44
扎心了，老铁
阅读(11688)
评论(2)
推荐(5)

Hive学习之路 （十九）Hive的数据倾斜

摘要：1、什么是数据倾斜？ 由于数据分布不均匀，造成数据大量的集中到一点，造成数据热点 2、Hadoop 框架的特性 A、不怕数据大，怕数据倾斜 B、Jobs 数比较多的作业运行效率相对比较低，如子查询比较多 C、 sum,count,max,min 等聚集函数，通常不会有数据倾斜问题 3、主要表现 任务
阅读全文

posted @ 2018-04-15 15:41
扎心了，老铁
阅读(33588)
评论(2)
推荐(4)

Hive学习之路 （十八）Hive的Shell操作

摘要：一、Hive的命令行 1、Hive支持的一些命令 Command Description quit Use quit or exit to leave the interactive shell. set key=value Use this to set value of particular c
阅读全文

posted @ 2018-04-15 15:40
扎心了，老铁
阅读(15295)
评论(0)
推荐(0)

Hive学习之路 （十七）Hive分析窗口函数(五) GROUPING SETS、GROUPING__ID、CUBE和ROLLUP

摘要：概述 GROUPING SETS,GROUPING__ID,CUBE,ROLLUP 这几个分析函数通常用于OLAP中，不能累加，而且需要根据不同维度上钻和下钻的指标统计，比如，分小时、天、月的UV数。 数据准备 数据格式 创建表 玩一玩GROUPING SETS和GROUPING__ID 说明 在一
阅读全文

posted @ 2018-04-15 15:37
扎心了，老铁
阅读(15391)
评论(1)
推荐(4)

Hive学习之路 （十六）Hive分析窗口函数(四) LAG、LEAD、FIRST_VALUE和LAST_VALUE

摘要：数据准备 数据格式 cookie4.txt 创建表 玩一玩LAG 说明 LAG(col,n,DEFAULT) 用于统计窗口内往上第n行值 第一个参数为列名，第二个参数为往上第n行（可选，默认为1），第三个参数为默认值（当往上第n行为NULL时候，...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
