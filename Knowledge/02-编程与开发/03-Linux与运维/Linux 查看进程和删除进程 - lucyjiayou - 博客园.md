---
url: http://www.cnblogs.com/lucyjiayou/archive/2012/02/24/2366194.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端, DevOps, 设计
content_type: article
word_count: 1713
status: 已处理
---

# Linux 查看进程和删除进程 - lucyjiayou - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/lucyjiayou/archive/2012/02/24/2366194.html)

## 核心要点

1. lucyjiayou

学习笔记

博客园

首页

新随笔

联系

管理

订阅

 Linux 查看进程和删除进程

1
2. 在 LINUX 命令平台输入 1-2 个字符后按 Tab 键会自动补全后面的部分(前提是要有这个东西，例如在装了 tomcat 的前提下, 输入 tomcat 的 to 按 tab)
3. grep 是搜索
例如： ps -ef | grep java
表示查看所有进程里 CMD 是 java 的进程信息
ps -aux | grep java
-aux 显示所有状态
ps
3

## 内容预览

lucyjiayou

学习笔记

博客园

首页

新随笔

联系

管理

订阅

 Linux 查看进程和删除进程

1. 在 LINUX 命令平台输入 1-2 个字符后按 Tab 键会自动补全后面的部分(前提是要有这个东西，例如在装了 tomcat 的前提下, 输入 tomcat 的 to 按 tab)。
2. ps 命令用于查看当前正在运行的进程。
grep 是搜索
例如： ps -ef | grep java
表示查看所有进程里 CMD 是 java 的进程信息
ps -aux | grep java
-aux 显示所有状态
ps
3. kill 命令用于终止进程
例如： kill -9 [PID]
-9 表示强迫进程立即停止
通常用 ps 查看进程 PID ，用 kill 命令终止进程
网上关于这两块的内容
-----------------------------------------------------------------------------------
PS
-----------------------------------------------------------------------------------
1. ps 简介
ps 命令就是最根本相应情况下也是相当强大地进程查看命令.运用该命令可以确定有哪些进程正在运行和运行地状态、...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
