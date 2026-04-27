---
url: https://segmentfault.com/a/1190000008700555
source: segmentfault.com
date_saved: 2026-04-23
tags: 数据库, DevOps, 算法
content_type: article
word_count: 1974
status: 已处理
---

# php - MySQL 字符集问题 - 个人文章 - SegmentFault 思否

> source: [segmentfault.com](https://segmentfault.com/a/1190000008700555)

## 核心要点

1. MySQL 字符集问题

melody_lql

2017-03-15 阅读 2 分钟

1

最近公司一个旧的项目需要支持 emoji 表情，一开始以为只要修改下数据库的表字段就好，没想到引发了一系列的问题。这里总结下，以作备忘
2. 将操作结果从内部操作字符集转换为 character_set_results, 响应请求
3. 连接数据库的字符集也需要是utf8mb4

character_set_server、character_set_database 等默认字符集的类型并没有那么重要，但最好还是保持一致

TP 的坑果然是多，远离TP

参考

http://www.laruence.com/2008/..

## 内容预览

MySQL 字符集问题

melody_lql

2017-03-15 阅读 2 分钟

1

最近公司一个旧的项目需要支持 emoji 表情，一开始以为只要修改下数据库的表字段就好，没想到引发了一系列的问题。这里总结下，以作备忘。

01 MySQL 字符集设置

系统变量：

character_set_server： 默认的内部操作字符集
character_set_client： 客户端来源数据使用的字符集
character_set_connection：连接层字符集
character_set_results： 查询结果字符集
character_set_database： 当前选中数据库的默认字符集
character_set_system： 系统元数据(字段名等)字符集

02 MySQL 中的字符集转换过程

MySQL Server收到请求时将请求数据从 character_set_client 转换为character_set_connection；

进行内部操作前将请求数据从 character_set_connection 转换为内部操作字符集，其确定方法如下：

使用表中字段的 CHARACTER SET 设定值；

若上述值不存在，则使用对应数据表的 DEFAULT CHARACTER SET 设定值(MySQL扩展，非SQL标准)；

若上述值不存...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
