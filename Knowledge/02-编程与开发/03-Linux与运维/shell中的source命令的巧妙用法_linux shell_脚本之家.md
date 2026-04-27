---
url: http://www.jb51.net/article/128917.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: Python, 后端, DevOps
content_type: article
word_count: 3487
status: 已处理
---

# shell中的source命令的巧妙用法_linux shell_脚本之家

> source: [www.jb51.net](http://www.jb51.net/article/128917.htm)

## 核心要点

1. filepath，sh filepath或者./filepath区别：

1
2. sh filepath会重新建立一个子shell，在子shell中执行脚本里面的语句，该子shell继承父shell的环境变量，但子shell是新建的，其改变的变量不会被带回父shell，除非使用export
3. source filename其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell。那么脚本里面所有新建、改变变量的语句都会保存在当前shell里面

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

 您的位置：首页 → 脚本专栏 → linux shell → shell source 命令用法

 shell中的source命令的巧妙用法

  更新时间：2023年08月01日 11:26:06   作者：linux_c_coding_man   

 这篇文章主要介绍了shell中的source命令的巧妙用法,本文给大家介绍的非常详细，对大家的学习或工作具有一定的参考借鉴价值，需要的朋友可以参考下

 首先，通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录。例如，当我们修改了/etc/profile文件，并想让它立刻生效，而...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
