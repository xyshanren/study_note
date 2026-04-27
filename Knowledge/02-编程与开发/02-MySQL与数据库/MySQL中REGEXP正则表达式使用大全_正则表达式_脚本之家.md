---
url: http://www.jb51.net/article/72928.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: 前端, 后端, 数据库
content_type: article
word_count: 6000
status: 已处理
---

# MySQL中REGEXP正则表达式使用大全_正则表达式_脚本之家

> source: [www.jb51.net](http://www.jb51.net/article/72928.htm)

## 核心要点

1. MySQL采用Henry Spencer的正则表达式实施，其目标是符合POSIX 1003.2。请参见附录C：感谢。MySQL采用了扩展的版本，以支持在SQL语句中与REGEXP操作符一起使用的模式匹配操作。请参见3.3.4.7节，“模式匹配”
2. 在本附录中，归纳了在MySQL中可用于REGEXP操作的特殊字符和结构，并给出了一些示例。本附录未包含可在Henry Spencer的regex(7)手册页面中发现的所有细节。该手册页面包含在MySQL源码分发版中，位于regex目录下的regex.7文件中
3. 正则表达式描述了一组字符串。最简单的正则表达式是不含任何特殊字符的正则表达式。例如，正则表达式hello匹配hello

## 内容预览

脚本之家

 服务器常用软件

 手机版

 关注微信

 快捷导航 

 网站首页

 网页制作

 网络编程

 脚本专栏

 脚本下载

 数据库

 服务器

 电子书籍

 操作系统

 网站运营

 平面设计

 其它

 媒体动画

 电脑基础

 硬件教程

 网络安全

 JavaScript

ASP.NET

PHP编程

AJAX相关

正则表达式

ASP编程

JSP编程

编程10000问

CSS/HTML

Flex

脚本加解密

web2.0

XML/RSS

网页编辑器

相关技巧

安全相关

网页播放器

其它综合

Dart

 您的位置：首页 → 网络编程 → 正则表达式 → MySQL中REGEXP正则表达式

 MySQL中REGEXP正则表达式使用大全

  更新时间：2015年09月29日 11:28:37   投稿：mrr   

 使用正则表达式操作mysql数据库非常方便，本篇文章给大家分享mysql中REGEXP正则表达式使用大全，感兴趣的朋友跟着小编一起看看吧

 以前我要查找数据都是使用like后来发现mysql中也有正则表达式了并且感觉性能要好于like，下面我来给大家分享一下mysql REGEXP正则表达式使用详解，希望此方法对大家有帮助。

在开始这个话题之前我们首...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
