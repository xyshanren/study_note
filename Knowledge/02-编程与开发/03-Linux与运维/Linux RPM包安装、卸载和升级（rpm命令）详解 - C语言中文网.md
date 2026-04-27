---
url: http://c.biancheng.net/view/2872.html
source: c.biancheng.net
date_saved: 2026-04-23
tags: Python, 后端, DevOps
content_type: article
word_count: 4725
status: 已处理
---

# Linux RPM包安装、卸载和升级（rpm命令）详解 - C语言中文网

> source: [c.biancheng.net](http://c.biancheng.net/view/2872.html)

## 核心要点

1. 我们以安装 apache 程序为例。因为后续章节还会介绍使用源码包的方式安装 apache 程序，读者可以直观地感受到源码包和 RPM 包的区别
2. RPM包默认安装路径

通常情况下，RPM 包采用系统默认的安装路径，所有安装文件会按照类别分散安装到表 1 所示的目录中
3. 除此之外，RPM 包也支持手动指定安装路径，但此方式并不推荐。因为一旦手动指定安装路径，所有的安装文件会集中安装到指定位置，且系统中用来查询安装路径的命令也无法使用（需要进行手工配置才能被系统识别），得不偿失

## 内容预览

首页

 C语言教程

 C++教程

 Python教程

 Java教程

 Linux入门

 更多>>

 目录 

 Linux

 1
 Linux简介

 2
 Linux安装

 3
 Linux文件和目录管理

 4
 Linux打包（归档）和压缩

 5
 Vim文本编辑器

 6
 Linux文本处理（Linux三剑客）

 7 Linux软件安装 7.1 Linux软件包7.2 Linux RPM包统一命名规则7.3 Linux RPM包安装、卸载和升级7.4 Linux rpm命令查询软件包7.5 Linux RPM包验证和数字证书7.6 Linux提取RPM包7.7 Linux SRPM源码包安装7.8 Linux重建RPM数据库（修复损坏的RPM数据库）7.9 RPM包的依赖性及其解决方案7.10 Linux yum源及配置7.11 Linux yum命令7.12 Linux yum管理软件组7.13 Linux源码包安装和卸载7.14 Linux源码包升级7.15 RPM包和源码包，究竟应该选择哪种安装方式？7.16 Linux函数库（静态函数库和动态函数库）及其安装过程7.17 Linux脚本程序包及安装方法详解（以webmin为例）

 8
 Linux用户和用户组管理

 9
 Linux权限管理

 10
 Linux文件系统管理...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
