---
url: https://juejin.im/post/6844903713048363021
source: juejin.im
date_saved: 2026-04-23
tags: Python, 数据库, DevOps
content_type: article
word_count: 6000
status: 已处理
---

# 介绍一个 MySQL 自动化运维利器 - Inception最近打算做一个 MySQL 的数据库运维平台。这里面有一个非 - 掘金

> source: [juejin.im](https://juejin.im/post/6844903713048363021)

## 核心要点

1. 介绍一个 MySQL 自动化运维利器 - Inception

 杏仁技术站

 2018-11-13

 5,623

 阅读7分钟

作者：黄超

杏仁运维工程师，关注容器技术和自动化运维
2. 执行流程图如下：

安装 Inception

我安装的环境

OS: Ubuntu 16.04.2 LTS

安装依赖

下载 bison： 版本最好是2.6之前的（Ubuntu 16.04.2 LTS 版本下安装的是 bison-2.5.1），最新的可能会有问题，下载之后，需要自己编译源码来安装，具体安装方法，可以参数网上的一些说明
3. 启动 Inception

创建一个配置文件 inc.cnf, 里面主要是配置 Inception 启动的端口，SQL 审核的策略，备份数据库的配置等等，更多可参考官方文档

## 内容预览

介绍一个 MySQL 自动化运维利器 - Inception

 杏仁技术站

 2018-11-13

 5,623

 阅读7分钟

作者：黄超

杏仁运维工程师，关注容器技术和自动化运维。

引子

最近打算做一个 MySQL 的数据库运维平台。这里面有一个非常重要的功能就是 SQL 的审核，如果完全靠人工去实现就没必要做成一个平台了。正没头绪如何去实现的时候，google 了一下，看下有没有现成的开源方案。果不其然，github 上发现一个『去哪儿网』开源的一个数据库运维工具 Inception, 它是一个集审核、执行、备份及生成回滚语句于一身的 MySQL 自动化运维工具。

Inception 介绍

Inception 的架构图如下图所示，简单来说，Inception 就是一个 MySQL 的代理，能够帮助你审核 SQL，执行 SQL，备份 SQL 影响的记录。Inception 是一个 C/S 的软件架构。我们可以通过原生的 MySQL 客户端 去连接，也可以通过远程的接口去连接，目前执行只支持通过C/C++接口、Python接口来对Inception访问。

执行流程图如下：

安装 Inception

我安装的环境

OS: Ubuntu 16.04.2 LTS

安装依赖

下载 bison： 版本最好是2.6之前的（Ubuntu 16.04.2 LTS 版...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
