---
url: http://dblab.xmu.edu.cn/blog/install-sqoop1/
source: dblab.xmu.edu.cn
date_saved: 2026-04-23
tags: 后端, 数据库, DevOps
content_type: article
word_count: 2241
status: 已处理
---

# Ubuntu安装Sqoop_厦大数据库实验室博客

> source: [dblab.xmu.edu.cn](http://dblab.xmu.edu.cn/blog/install-sqoop1/)

## 核心要点

1. Sqoop是一款开源的工具，主要用于在Hadoop(Hive)与传统的数据库(mysql、postgresql.
2. )间进行数据的传递，可以将一个关系型数据库（例如 ： MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。
3. Sqoop项目开始于2009年，最早是作为Hadoop的一个第三方模块存在，后来为了让使用者能够快速部署，也为了让开发人员能够更快速的迭代开发，Sqoop独立成为一个Apache项目。
4. export HADOOP_COMMON_HOME=/usr/local/hadoop
5. export HADOOP_MAPRED_HOME=/usr/local/hadoop
## 内容预览

厦大数据库实验室博客

总结、分享、收获实验室主页

搜索：

﻿

Sqoop是一款开源的工具，主要用于在Hadoop(Hive)与传统的数据库(mysql、postgresql...)间进行数据的传递，可以将一个关系型数据库（例如 ： MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。Sqoop项目开始于2009年，最早是作为Hadoop的一个第三方模块存在，后来为了让使用者能够快速部署，也为了让开发人员能够更快速的迭代开发，Sqoop独立成为一个Apache项目。

安装环境：

操作系统：Linux系统(Ubuntu14.04)

sqoop版本：1.4.6

Hadoop：2.7.2

MySQL：5.7.15

注意：sqoop1与sqoop2完全不兼容，1.4.6及之前的版本是sqoop1，之后的是sqoop2

1 下载并解压sqoop1.4.6

请登录Linux系统（本教程是使用hadoop用户名登录），然后，在Linux的浏览器（一般自带的是火狐Firefox浏览器）中，打开本网页，点击sqoop下载地址，下载Sqoop安装文件sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz。浏览器默认会被下载文件保存到当前登录用户的下载目录下面。

下面执行以下命令

cd ~ #进入当前用户的用户目录
cd 下载 #sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz文件下载后就被保存在该目录下面
sudo tar -zxvf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz -C /usr/local #解压安装文件
cd /usr/local
sudo mv sqoop-1.4.6.bin__hadoop-2.0.4-alpha sqoop #修改文件名
sudo chown -R hadoop:hadoop sqoop #修改文件夹属主，如果你当前登录用户名不是hadoop，请修改成你自己的用户名

2 修改配置文件sqoop-env.sh

cd sqoop/conf/
cat sqoop-env-template.sh >> sqoop-env.sh #将sqoop-env-template.sh复制一份并命名为sqoop-env.sh
vim sqoop-env.sh #编辑sqoop-env.sh

修改sqoop-env.sh的如下信息

export HADOOP_COMMON_HOME=/usr/local/hadoop
export HADOOP_MAPRED_HOME=/usr/local/hadoop
export HBASE_HOME=/usr/local/hbase
export HIVE_HOME=/usr/local/hive
#export ZOOCFGDIR= #如果读者配置了ZooKeeper,也需要在此配置ZooKeeper的路径

3 配置环境变量

打开当前用户的环境变量配置文件：

vim ~/.bashrc

在配置文件第一行键入如下信息：

export SQOOP_HOME=/usr/local/sqoop
export PATH=$PATH:$SBT_HOME/bin:$SQOOP_HOME/bin
export CLASSPATH=$CLASSPATH:$SQOOP_HOME/lib

保存...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
