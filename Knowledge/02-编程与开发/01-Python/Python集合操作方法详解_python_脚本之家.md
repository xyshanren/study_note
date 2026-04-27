---
url: https://www.jb51.net/article/179895.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: Python, 后端, DevOps
content_type: article
word_count: 6439
status: 已处理
---

# Python集合操作方法详解_python_脚本之家

> source: [www.jb51.net](https://www.jb51.net/article/179895.htm)

## 核心要点

1. 您的位置：首页 → 脚本专栏 → python → Python集合操作方法
2. 更新时间：2020年02月09日 13:18:19 作者：罗阿红
3. 这篇文章主要介绍了Python集合操作方法详解,需要的朋友可以参考下
4. >>> name_1 = [1,2,3,4,7,8,7,10]
5. {1, 2, 3, 4, 7, 8, 10} 
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

您的位置：首页 → 脚本专栏 → python → Python集合操作方法

Python集合操作方法详解

更新时间：2020年02月09日 13:18:19 作者：罗阿红

这篇文章主要介绍了Python集合操作方法详解,需要的朋友可以参考下

集合是无序的，天生不重复的数据组合，它的作用如下：

去重，即：把一个列表变成集合，就去重了

关系测试，即：测试两组集合的交集、并集和差集等

一、集合常用方法总结

二、定义

1、语法

>>> name_1 = [1,2,3,4,7,8,7,10]

#把列表转换为集合

>>> name_1 = set(name_1)

#转换后，去重

>>> print(name_1,type(name_1))

{1, 2, 3, 4, 7, 8, 10} 

三、关系测试

1、交集(intersection())

>>> name_1 = [1,2,3,4,7,8,7,10]

>>> name_2 = [1,3,5,8,10]

>>> name_1 = set(name_1)

>>> name_2 = set(name_2)

#输出结果

>>> name_1.intersection(name_2)

{8, 1, 10, 3}

2、并集(union())

>>> name_1 = [1,2,3,4,7,8,7,10]

>>> name_2 = [1,3,5,8,10]

>>> name_1 = set(name_1)

>>> name_2 = set(name_2)

#输出结果

>>> name_1.union(name_2)

{1, 2, 3, 4, 5, 7, 8, 10}

3、差集(difference())

>>> name_1 = [1,2,3,4,7,8,7,10]

>>> name_2 = [1,3,5,8,10]

>>> name_1 = set(name_1)

>>> name_2 = set(name_2)

#输出结果

>>> name_1.difference(name_2)

{2, 4, 7}

特别提示：差集取的是数值在第一个集合中，但是不在第二个集合中（在我不在你）

4、issubset()

判断一个集合是否是另一个集合的子集

>>> name_1 = [1,2,3,4,7,8,7,10]

>>> name_3 = [1,2,3,4]

>>> name_1 = set(name_1)

>>> name_3 = set(name_3)

#输出结果

>>> name_3.issubset(name_1)

True

5、issuperset()

判断一个集合是否是另一个集合的父集

>>> name_1 = [1,2,3,4,7,8,7...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
