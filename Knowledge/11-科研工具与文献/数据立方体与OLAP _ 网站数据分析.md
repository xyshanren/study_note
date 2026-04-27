---
url: http://webdataanalysis.net/web-data-warehouse/data-cube-and-olap/
source: webdataanalysis.net
date_saved: 2026-04-23
tags: 效率工具
content_type: article
word_count: 3812
status: 已处理
---

# 数据立方体与OLAP | 网站数据分析

> source: [webdataanalysis.net](http://webdataanalysis.net/web-data-warehouse/data-cube-and-olap/)

## 核心要点

1. MOLAP(Multidimensional)

　　即基于多维数组的存储模型，也是最原始的OLAP，但需要对数据进行预处理才能形成多维结构
2. ROLAP(Relational)

　　比较常见的OLAP类型，这里介绍和讨论的也基本都是ROLAP类型，可以从多维数据模型的那篇文章的图中看到，其实ROLAP是完全基于关系模型进行存放的，只是它根据分析的需要对模型的结构和组织形式进行的优化，更利于OLAP
3. HOLAP(Hybrid)

　　介于MOLAP和ROLAP的类型，我的理解是细节的数据以ROLAP的形式存放，更加方便灵活，而高度聚合的数据以MOLAP的形式展现，更适合于高效的分析处理

## 内容预览

前面的一篇文章——数据仓库的多维数据模型中已经简单介绍过多维模型的定义和结构，以及事实表（Fact Table）和维表（Dimension Table）的概念。多维数据模型作为一种新的逻辑模型赋予了数据新的组织和存储形式，而真正体现其在分析上的优势还需要基于模型的有效的操作和处理，也就是OLAP（On-line Analytical Processing，联机分析处理）。

数据立方体

　　关于数据立方体（Data Cube），这里必须注意的是数据立方体只是多维模型的一个形象的说法。立方体其本身只有三维，但多维模型不仅限于三维模型，可以组合更多的维度，但一方面是出于更方便地解释和描述，同时也是给思维成像和想象的空间；另一方面是为了与传统关系型数据库的二维表区别开来，于是就有了数据立方体的叫法。所以本文中也是引用立方体，也就是把多维模型以三维的方式为代表进行展现和描述，其实上Google图片搜索“OLAP”会有一大堆的数据立方体图片，这里我自己画了一个：

OLAP

　　OLAP（On-line Analytical Processing，联机分析处理）是在基于数据仓库多维模型的基础上实现的面向分析的各类操作的集合。可以比较下其与传统的OLTP（On-line Transaction Processing，联机事务处理）的区别来看一下它的特点：

OLAP与OLTP

数据处理类...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
