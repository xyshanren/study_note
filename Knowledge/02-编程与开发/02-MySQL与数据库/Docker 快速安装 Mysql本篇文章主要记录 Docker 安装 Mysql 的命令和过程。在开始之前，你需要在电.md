---
url: https://juejin.cn/post/6872210647287857165
source: juejin.cn
date_saved: 2026-04-23
tags: 数据库, DevOps, 学习-教程
content_type: article
word_count: 3211
status: 已处理
---

# Docker 快速安装 Mysql本篇文章主要记录 Docker 安装 Mysql 的命令和过程。在开始之前，你需要在电 - 掘金

> source: [juejin.cn](https://juejin.cn/post/6872210647287857165)

## 核心要点

1. 配置阿里云镜像加速

Docker 镜像源在国外，国内访问速度非常慢，要想获得更高的下载速度需要配置国内镜像源
2. 可以等待一段时间后运行该命令，因为 Docker 容器启动需要一定的时间

## 内容预览

Docker 快速安装 Mysql

 全栈港

 2020-09-14

 11,504

 阅读1分钟

 本篇文章主要记录 Docker 安装 Mysql 的命令和过程。在开始之前，你需要在电脑上安装 Docker 环境，可参考

在 CentOS 系统上安装 Docker Engine

在 Ubuntu 上安装 Docker Engine

本教程适应于 CentOS 与 Ubuntu 系统。

配置阿里云镜像加速

Docker 镜像源在国外，国内访问速度非常慢，要想获得更高的下载速度需要配置国内镜像源。

进入阿里云镜像加速配置页面 cr.console.aliyun.com/cn-hangzhou… （需要登录）获取你账号下的加速地址

通过修改 daemon 配置文件 /etc/docker/daemon.json 来使用加速器

如下是 CentOs、Ubuntu 系统的配置命令，请将镜像加速地址（代码中的不可用）换成你自己的地址 罒ω罒

sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json -'EOF'
{
 "registry-mirrors": ["https://kwli5l3lxi5a0mx.mirror.aliyuncs.com"]
}
EOF
sudo sys...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
