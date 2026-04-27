---
url: http://blog.csdn.net/allenalex/article/details/39084421
source: blog.csdn.net
date_saved: 2026-04-23
tags: 后端, DevOps, 设计
content_type: article
word_count: 6000
status: 已处理
---

# linux下部署和执行kettle——（配置文件、动态获取、后台执行）_linux kettle.properties-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/allenalex/article/details/39084421)

## 核心要点

1. kettle
 专栏收录该内容

 1 篇文章

 订阅专栏

 本文详细介绍Kettle在Linux环境下的部署步骤，包括JDK配置、Kettle安装、Shell脚本执行Job或转换，以及环境变量配置等内容。此外，还介绍了如何通过配置文件实现参数动态化，以及后台执行Job的方法
2. 一.部署准备

1.1 java安装略

1.2 JDK配置

        1
3. 命令行键入“vi

 profile”打开profile文件

3

## 内容预览

linux下部署和执行kettle——（配置文件、动态获取、后台执行）

 最新推荐文章于 2026-02-16 01:04:11 发布

 原创

 最新推荐文章于 2026-02-16 01:04:11 发布
 ·
 3.7w 阅读

 ·

 11

 ·

 26

 ·

 CC 4.0 BY-SA版权

 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

 kettle
 专栏收录该内容

 1 篇文章

 订阅专栏

 本文详细介绍Kettle在Linux环境下的部署步骤，包括JDK配置、Kettle安装、Shell脚本执行Job或转换，以及环境变量配置等内容。此外，还介绍了如何通过配置文件实现参数动态化，以及后台执行Job的方法。

一.部署准备

1.1 java安装略

1.2 JDK配置

        1.     命令行键入“cd

 /etc”进入etc目录

2.     命令行键入“vi

 profile”打开profile文件

3.     敲击键盘ctrlF到文件末尾

4.     在末尾处即第一个~的地方敲击键盘将以下内容输入到文件

export JAVA_HOME...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
