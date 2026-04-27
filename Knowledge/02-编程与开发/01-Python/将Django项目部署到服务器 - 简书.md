---
url: https://www.jianshu.com/p/36ef6557c910
source: www.jianshu.com
date_saved: 2026-04-23
tags: Python, 后端, 数据库
content_type: article
word_count: 5632
status: 已处理
---

# 将Django项目部署到服务器 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/36ef6557c910)

## 核心要点

1. 将Django项目部署到服务器
一、概述
在本地搭建好了个人博客之后，接着需要将其部署到服务器上面。在此之前，我们需要做一些准备工作
2. 1.1 准备工具
部署网站到服务器之前，我们需要拥有一个云主机与域名，这两者都需要购买。目前已经拥有
3. 1.2 相关基础概念
在部署网站到服务器上面的过程中，涉及到一些新的东西，要理解其作用。参考文章《web建站过程中流程》。现在基本知道了部署网站所需要的各项东西，这个个人博客的架构是Linux + Nginx+MySQL+Python，Framework是Django

## 内容预览

将Django项目部署到服务器
一、概述
在本地搭建好了个人博客之后，接着需要将其部署到服务器上面。在此之前，我们需要做一些准备工作。
1.1 准备工具
部署网站到服务器之前，我们需要拥有一个云主机与域名，这两者都需要购买。目前已经拥有。
1.2 相关基础概念
在部署网站到服务器上面的过程中，涉及到一些新的东西，要理解其作用。参考文章《web建站过程中流程》。现在基本知道了部署网站所需要的各项东西，这个个人博客的架构是Linux + Nginx+MySQL+Python，Framework是Django。
二、流程图解

流程图

具体流程：
当客户端访问服务器的时候，其实是浏览器和服务器上的 Web Server（Web服务）产生了一个HTTP协议的通信。上图Python编写的源代码是运行在Python运行环境中的，并且是封装子Python的虚拟环境venv中。
接下来用到了uWSGI模块，这是属于Python的模块，（此模块在Win上运行错误，在Linux上运行良好）。uWSGI一方面通过WSGI与Python代码进行通信，另一方面通过socket（套接字）与Nginx通信。
三、WSGI
3.1 WSGI 是什么？
WSGI 的全称是Web Server Gateway Interface ,翻译过来就是Web服务器网关接口。具体来说，
WSGI是一个规范，定义了Web服务器...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
