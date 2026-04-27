---
url: http://blog.csdn.net/m0_37645820/article/details/75268972
source: blog.csdn.net
date_saved: 2026-04-23
tags: 数据库, 学习-教程
content_type: article
word_count: 6000
status: 已处理
---

# MySQL中REGEXP正则表达式使用大全_mysql regex-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/m0_37645820/article/details/75268972)

## 核心要点

1. 以前我要查找数据都是使用like后来发现mysql中也有正则表达式了并且感觉性能要好于like下面我来给大家分享一下mysql REGEXP正则表达式使用详解希望此方法对大家有帮助
2. MySQL采用Henry Spencer的正则表达式实施其目标是符合POSIX 1003.2。请参见附录C感谢。MySQL采用了扩展的版本以支持在SQL语句中与REGEXP操作符一起使用的模式匹配操作。请参见3.3.4.7节“模式匹配”
3. 在本附录中归纳了在MySQL中可用于REGEXP操作的特殊字符和结构并给出了一些示例。本附录未包含可在Henry Spencer的regex(7)手册页面中发现的所有细节。该手册页面包含在MySQL源码分发版中位于regex目录下的regex.7文件中

## 内容预览

MySQL中REGEXP正则表达式使用大全

 最新推荐文章于 2026-03-23 00:45:21 发布

 转载

 最新推荐文章于 2026-03-23 00:45:21 发布
 ·
 1.2w 阅读

 ·

 4

 ·

 13

 数据库学习
 专栏收录该内容

 18 篇文章

 订阅专栏

 本文介绍了MySQL中正则表达式的使用方法，包括各种符号的意义及应用场景，通过具体示例展示了如何进行数据验证和筛选。

以前我要查找数据都是使用like后来发现mysql中也有正则表达式了并且感觉性能要好于like下面我来给大家分享一下mysql REGEXP正则表达式使用详解希望此方法对大家有帮助。

MySQL采用Henry Spencer的正则表达式实施其目标是符合POSIX 1003.2。请参见附录C感谢。MySQL采用了扩展的版本以支持在SQL语句中与REGEXP操作符一起使用的模式匹配操作。请参见3.3.4.7节“模式匹配”。

在本附录中归纳了在MySQL中可用于REGEXP操作的特殊字符和结构并给出了一些示例。本附录未包含可在Henry Spencer的regex(7)手册页面中发现的所有细节...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
