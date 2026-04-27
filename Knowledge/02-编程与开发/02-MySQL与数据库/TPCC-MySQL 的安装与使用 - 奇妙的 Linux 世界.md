---
url: https://www.hi-linux.com/posts/38534.html#vip-container
source: www.hi-linux.com
date_saved: 2026-04-23
tags: 数据库, DevOps, 设计
content_type: article
word_count: 6000
status: 已处理
---

# TPCC-MySQL 的安装与使用 - 奇妙的 Linux 世界

> source: [www.hi-linux.com](https://www.hi-linux.com/posts/38534.html#vip-container)

## 核心要点

1. 什么是TPC-C

TPC-C是专门针对联机交易处理系统（OLTP系统）的规范，一般情况下我们也把这类系统称为业务处理系统
2. TPC-C是TPC(Transaction Processing Performance Council)组织发布的一个测试规范，用于模拟测试复杂的在线事务处理系统。其测试结果包括每分钟事务数(tpmC)，以及每事务的成本(Price/tpmC)。在进行大压力下MySQL的一些行为时经常使用
3. 什么是TPCC-MYSQL

TPCC-MYSQL是percona基于TPC-C(下面简写成TPCC)衍生出来的产品，专用于MySQL基准测试。用来测试数据库的压力工具，模拟一个电商的业务，主要的业务有新增订单，库存查询，发货，支付等模块的测试

## 内容预览

什么是TPC-C

TPC-C是专门针对联机交易处理系统（OLTP系统）的规范，一般情况下我们也把这类系统称为业务处理系统。

TPC-C是TPC(Transaction Processing Performance Council)组织发布的一个测试规范，用于模拟测试复杂的在线事务处理系统。其测试结果包括每分钟事务数(tpmC)，以及每事务的成本(Price/tpmC)。在进行大压力下MySQL的一些行为时经常使用。

什么是TPCC-MYSQL

TPCC-MYSQL是percona基于TPC-C(下面简写成TPCC)衍生出来的产品，专用于MySQL基准测试。用来测试数据库的压力工具，模拟一个电商的业务，主要的业务有新增订单，库存查询，发货，支付等模块的测试。

percona官方版本

安装依赖包

MySQL 5.1

1
yum install mysql-devel

MySQL 5.6

1
yum install mysql-community-devel

编译安装

1
2
3
git clone https://github.com/Percona-Lab/tpcc-mysql
cd tpcc-mysql/src
make

如果make没报错，就会在tpcc-mysql文件夹下生成tpcc二进制命令行工具tpcc_load、tpcc_start

使用

t...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
