---
url: https://www.cnblogs.com/zhilingege/p/8921211.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 学习-教程, 效率工具
content_type: article
word_count: 1722
status: 已处理
---

# vscode sync插件 在不同设备 同步的坑 - 志林哥哥 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/zhilingege/p/8921211.html)

## 核心要点

1. 志林哥哥

博客园

首页

新随笔

联系

订阅

管理

 vscode sync插件 在不同设备 同步的坑

 Sync : Invalid / Expired GitHub Token
2. Please generate new token with scopes mentioned in readme
3. Please generate new token with scopes mentioned in readme

## 内容预览

志林哥哥

博客园

首页

新随笔

联系

订阅

管理

 vscode sync插件 在不同设备 同步的坑

 Sync : Invalid / Expired GitHub Token. Please generate new token with scopes mentioned in readme. Exception Logged in Console.

 sync的好处不言而喻，在不同的设备都可以同步自己的插件和所有配置；

但是有时有总是会有坑，

现在把我遇到的坑记录下来，以防再次踩坑

VSCode 同步方案

VSCode 的插件 Setting Sync 提供了通过 github 的 Gist 完成配置同步的功能。但是由于它的教程不完整，导致同步起来会产生省问题。最常见的问题是无法下载配置，提示信息为：

Sync : Invalid / Expired GitHub Token. Please generate new token with scopes mentioned in readme. Exception Logged in Console.

Gist 可以保存上传的配置文件。拉取配置文件需要配置两个 id，一个是 Gist Id，一个是 Token Id。这两个 Id 前者标识配置文件，后者用于身份验证。我们无法下载的原因就是...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
