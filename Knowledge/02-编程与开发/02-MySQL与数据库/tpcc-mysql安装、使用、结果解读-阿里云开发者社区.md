---
url: https://yq.aliyun.com/articles/178251
source: yq.aliyun.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 3235
status: 已处理
---

# tpcc-mysql安装、使用、结果解读-阿里云开发者社区

> source: [yq.aliyun.com](https://yq.aliyun.com/articles/178251)

## 核心要点

1. 侵权投诉表单进行举报，一经查实，本社区将立刻删除涉嫌侵权内容。
2. RDS DuckDB + QuickBI 企业套餐，8核32GB + QuickBI 专业版
3. RDS MySQL DuckDB 分析主实例，集群系列 4核8GB
4. TPC-C是专门针对联机交易处理系统（OLTP系统）的规范，一般情况下我们也把这类系统称为业务处理系统。
5. tpcc-mysql是percona基于TPC-C(下面简写成TPCC)衍生出来的产品，专用于MySQL基准测试。
## 内容预览

开发者社区
数据库
文章
正文

tpcc-mysql安装、使用、结果解读

2017-08-02
5371

版权

版权声明：

本文内容由阿里云实名注册用户自发贡献，版权归原作者所有，阿里云开发者社区不拥有其著作权，亦不承担相应法律责任。具体规则请查看《
阿里云开发者社区用户服务协议》和
《阿里云开发者社区知识产权保护指引》。如果您发现本社区中有涉嫌抄袭的内容，填写
侵权投诉表单进行举报，一经查实，本社区将立刻删除涉嫌侵权内容。

本文涉及的产品

RDS DuckDB + QuickBI 企业套餐，8核32GB + QuickBI 专业版

RDS AI 助手，专业版

RDS MySQL DuckDB 分析主实例，集群系列 4核8GB

简介：

1
关于tpcc-mysql
TPC-C是专门针对联机交易处理系统（OLTP系统）的规范，一般情况下我们也把这类系统称为业务处理系统。 tpcc-mysql是percona基于TPC-C(下面简写成TPCC)衍生出来的产品，专用于MySQL基准测试。其源码放在launchpad上，用bazaar管理。

2
tpcc-mysql安装
launchpad上的项目可以使用bzr客户端(类似cvs/svn)将源码下载到本地：

cd /tmp bzr branch lp:~percona-dev/perconatools/tpcc-mysql

也可从MySQL中文网的百度云盘共享快速下载：

http://pan.baidu.com/s/1pJr19CR

在源码目录下，直接执行 make 即可完成编译。当然了，mysql lib包是需要提前安装，如果不在默认目录下的话，可以手工修改 Makefile，指定正确的 mysql_config 目录即可，让mysql_config自己去找到对应的 libs 和 include 路径。

3
tpcc-mysql相关数据表用途介绍
tpcc-mysql的业务逻辑及其相关的几个表作用如下：

New-Order：新订单，主要对应 new_orders 表 Payment：支付，主要对应 orders、history 表 Order-Status：订单状态，主要对应 orders、order_line 表 Delivery：发货，主要对应 order_line 表 Stock-Level：库存，主要对应 stock 表 其他相关表： 客户：主要对应 customer 表 地区：主要对应 district 表 商品：主要对应 item 表 仓库：主要对应 warehouse 表

4
开始测试
1、初始化表结构及测试数据

#创建tpcc100库，初始化DDL

mysqladmin create tpcc100 mysql -f tpcc100 < create_table.sql mysql -f tpcc100 < add_fkey_idx.sql

#初始化测试数据，加载100个仓库的测试数据，仓库数越大，测试数据也越大，整个过程也越慢，需要根据服务器配置适当调整

tpcc_load localhost tpcc tpcc_user tpcc_passwd 100

2、开始测试

#下面的例子中，模拟对100个仓库(-w 100)，并发128个线程(-c 128)，预热5分钟(-r 300)，持续压测1小时(-l 3600)

tpcc_start -hlocalhost -utpcc_user -ptpcc_password -d tpcc100...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
