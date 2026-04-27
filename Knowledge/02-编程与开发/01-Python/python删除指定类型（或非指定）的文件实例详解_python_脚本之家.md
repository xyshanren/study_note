---
url: https://www.jb51.net/article/68964.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: Python, 后端, DevOps
content_type: article
word_count: 4428
status: 已处理
---

# python删除指定类型（或非指定）的文件实例详解_python_脚本之家

> source: [www.jb51.net](https://www.jb51.net/article/68964.htm)

## 核心要点

1. listdir 是按命名规则，对文件夹和文件、统一采用深度优先搜索的方式，进行列举

而os.walk的标准例程一般是先遍历文件，后遍历文件夹
2. 学习要点：

os类的函数：

os.getenv()和os.putenv()函数分别用来读取和设置环境变量
3. os.linesep字符串给出当前平台使用的行终止符。例如，Windows使用'\r\n'，Linux使用'\n'而Mac使用'\r'

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

 vbs

DOS/BAT

hta

htc

python

perl

游戏相关

VBA

远程脚本

ColdFusion

ruby

autoit

seraphzone

PowerShell

linux shell

Lua

Golang

Erlang

其它

 您的位置：首页 → 脚本专栏 → python → python删除指定类型（或非指定）的文件

 python删除指定类型（或非指定）的文件实例详解

  更新时间：2015年07月06日 09:48:17   作者：viewcode   

 这篇文章主要介绍了python删除指定类型（或非指定）的文件,以实例形式较为详细的分析了Python删除文件的相关技巧,需要的朋友可以参考下

 本文实例分析了python删除指定类型（或非指定）的文件用法。分享给大家供大家参考。具体如下：

如下，删除目录下非源码文件

import os 

import string 

d...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
