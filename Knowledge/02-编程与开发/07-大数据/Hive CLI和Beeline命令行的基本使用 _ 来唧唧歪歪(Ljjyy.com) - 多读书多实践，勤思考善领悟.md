---
url: https://www.ljjyy.com/archives/2019/07/100395.html#2-1-HiveServer2
source: www.ljjyy.com
date_saved: 2026-04-23
tags: 后端
content_type: article
word_count: 6000
status: 已处理
---

# Hive CLI和Beeline命令行的基本使用 | 来唧唧歪歪(Ljjyy.com) - 多读书多实践，勤思考善领悟

> source: [www.ljjyy.com](https://www.ljjyy.com/archives/2019/07/100395.html#2-1-HiveServer2)

## 核心要点

1. Hive CLI和Beeline命令行的基本使用

 云计算

 beeline hadoop hive

 2019/07/09

 本文于2412天之前发表，文中内容可能已经过时
2. 1.3 执行SQL命令
在不进入交互式命令行的情况下，可以使用hive -e 执行SQL命令
3. 1.6 配置文件启动
使用-i可以在进入交互模式之前运行初始化脚本，相当于指定配置文件启动

## 内容预览

Hive CLI和Beeline命令行的基本使用

 云计算

 beeline hadoop hive

 2019/07/09

 本文于2412天之前发表，文中内容可能已经过时。

 一、Hive CLI
1.1 Help
使用hive -H或者 hive --help命令可以查看所有命令的帮助，显示如下：

1
2
3
4
5
6
7
8
9
10
11
12
13
usage: hive
 -d,--define <key=value> Variable subsitution to apply to hive 
 commands. e.g. -d A=B or --define A=B --定义用户自定义变量
 --database <databasename> Specify the database to use -- 指定使用的数据库
 -e <quoted-query-string> SQL from command line -- 执行指定的SQL
 -f <filename> SQL from files --执行SQL脚本
 -H,--help Print help information -- 打印帮助信息
 --hiveconf <property=value> Use value for ...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
