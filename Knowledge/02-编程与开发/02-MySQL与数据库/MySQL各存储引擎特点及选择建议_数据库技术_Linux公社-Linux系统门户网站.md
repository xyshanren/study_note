---
url: https://www.linuxidc.com/Linux/2015-12/126566.htm
source: www.linuxidc.com
date_saved: 2026-04-23
tags: 后端, 数据库, DevOps
content_type: article
word_count: 2413
status: 已处理
---

# MySQL各存储引擎特点及选择建议_数据库技术_Linux公社-Linux系统门户网站

> source: [www.linuxidc.com](https://www.linuxidc.com/Linux/2015-12/126566.htm)

## 核心要点

1. MySQL官方存储引擎比较常见的存储引擎有：Innodb、MyISAM、Memory、Archive、NDB、BDB，第三方比较有名的：TokuDB、Infobright、InnfiniDB、XtraDB（Innodb增强版本）
2. 特性MyISAMInnoDBMemoryArchiveNDBBDB
3. TokuDB：支持数据压缩，支持高速写入的一个引擎，但是不适合update多的场景
4. Infobright/InfiniDB：基于列存储的引擎，适用于OLAP环境，Infobright社区版只支持load data操作
5. 选择存储引擎及建议：根据不同的业务去选择适合业务的存储引擎，MySQL的存储引擎很多，不同的库，不同的表都支持选择不同的存储引擎，推荐同一个库用同一种存储引擎，因为不同存储引擎的表之间join操作比较慢
## 内容预览

手机版

你好，游客 登录

注册

搜索

首页Linux新闻Linux教程数据库技术Linux编程服务器应用Linux安全Linux下载Linux主题Linux壁纸Linux软件数码手机电脑

首页 → 数据库技术

背景：

阅读新闻

MySQL各存储引擎特点及选择建议

[日期：2015-12-21]

来源：Linux社区 

作者：会说话的鱼

[字体：大 中 小]

MySQL官方存储引擎比较常见的存储引擎有：Innodb、MyISAM、Memory、Archive、NDB、BDB，第三方比较有名的：TokuDB、Infobright、InnfiniDB、XtraDB（Innodb增强版本）

官方存储引擎的特点对比

MySQL存储引擎比较

特性MyISAMInnoDBMemoryArchiveNDBBDB

存储限制

No

64TB

Yes

No

Yes

No

事务

&radic;

&radic;

MVCC

&radic;

&radic;

&radic;

锁粒度

Table

Row

Table

Row

Row

Page

B树索引

&radic;

&radic;

&radic;

&radic;

&radic;

哈希索引

&radic;

&radic;

&radic;

全文索引

&radic;

5.6支持e文

集群索引

&radic;

数据缓存

&radic;

&radic;

&radic;

索引缓存

&radic;

&radic;

&radic;

&radic;

数据压缩

&radic;

&radic;

批量插入

高

相对低

高

非常高

高

高

内存消耗

低

高

中

低

高

低

外键支持

&radic;

复制支持

&radic;

&radic;

&radic;

&radic;

&radic;

&radic;

查询缓存

&radic;

&radic;

&radic;

&radic;

&radic;

&radic;

备份恢复

&radic;

&radic;

&radic;

&radic;

&radic;

&radic;

集群支持

&radic;

TokuDB：支持数据压缩，支持高速写入的一个引擎，但是不适合update多的场景

Infobright/InfiniDB：基于列存储的引擎，适用于OLAP环境，Infobright社区版只支持load data操作

选择存储引擎及建议：根据不同的业务去选择适合业务的存储引擎，MySQL的存储引擎很多，不同的库，不同的表都支持选择不同的存储引擎，推荐同一个库用同一种存储引擎，因为不同存储引擎的表之间join操作比较慢

常用推荐：Innodb，非特殊的场景，Innodb存储引擎一般都可以满足需求

如果有大数据写入批量读取操作：TokuDB

针对OLAP可以考虑使用InfiniDB/Infobright

如果针对数据量小要求速度快，无持久化要求：Memory

尽量不要选择MyISAM存储引擎：因为MyISAM存储引擎只能用的单个CPU，内存只能用到4个G，内存里只有索引，而且并发能力差。

本文永久更新链接地址：http://www.linuxidc.com/Linux/2015-12/126566.htm

利用系统缓存提高Po...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
