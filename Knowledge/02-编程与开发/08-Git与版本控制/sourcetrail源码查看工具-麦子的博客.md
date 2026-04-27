---
url: https://maizitoday.github.io/post/vscode%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7-sourcetrail%E6%BA%90%E7%A0%81%E6%9F%A5%E7%9C%8B%E5%B7%A5%E5%85%B7/
source: maizitoday.github.io
date_saved: 2026-04-23
tags: 后端, 数据库, DevOps
content_type: article
word_count: 1509
status: 已处理
---

# sourcetrail源码查看工具-麦子的博客

> source: [maizitoday.github.io](https://maizitoday.github.io/post/vscode%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7-sourcetrail%E6%BA%90%E7%A0%81%E6%9F%A5%E7%9C%8B%E5%B7%A5%E5%85%B7/)

## 核心要点

1. Sourcetrail: (Re)start server
看到下面标志就表示成功了
2. 注意：如果没有成功，就重启一下vscode和客户端。我是重启后，他才自动连接成功的
3. 插件基本使用

黄色块 ：表示的是这个类中，调用了哪些方法和创建了哪些对象

## 内容预览

sourcetrail源码查看工具-麦子的博客

 [TOC]

sourcetrail插件配置

vscode配置

 "sourcetrail.startServerAtStartup": true, // 设置为true在VS Code启动时启动TCP侦听器
 "sourcetrail.ip": "127.0.0.1", // TCP服务器IP地址
 "sourcetrail.pluginPort": 8666, // 端口VS代码监听
 "sourcetrail.sourcetrailPort": 8667, // 端口Sourcetrail监听
客户端软件配置

执行步骤

安装好后，执行下面命令。

Sourcetrail: (Re)start server
看到下面标志就表示成功了。

就可以和sourcetrail客户端，进行代码的切换了。

注意：如果没有成功，就重启一下vscode和客户端。我是重启后，他才自动连接成功的。

插件基本使用

黄色块 ：表示的是这个类中，调用了哪些方法和创建了哪些对象。

小房子图标 ：表示的是这个对象的属性。

地球图标：表示的这个类里面的方法分别是什么。

黄色线条：表示的从哪一方法调用了这些方法。比如看实际代码，main方法里...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
