---
url: https://juejin.im/post/6844903794187173901
source: juejin.im
date_saved: 2026-04-23
tags: Python, 数据库, DevOps
content_type: article
word_count: 6000
status: 已处理
---

# 使用 Docker-compose 离线部署Django应用我们所在的内网环境需要部署一个类似CMS的应用，就是一些表格 - 掘金

> source: [juejin.im](https://juejin.im/post/6844903794187173901)

## 核心要点

1. 卸载旧版本

在安装之前需要卸载旧版本的 docker，如果你是新系统，可以忽略这一步
2. $ sudo apt update
$ sudo apt install -y docker-ce
安装完成后，启动 docker 服务并使其能够在每次系统引导时启动
3. $ sudo systemctl start docker
$ sudo systemctl enable docker
安装开发环境的Docker-compose

Docker-ce安装完成后，Docker-compose就好办了。如果你是在Linux等平台上直接下载Docker-compose的编译好的二进制文件即可使用

## 内容预览

使用 Docker-compose 离线部署Django应用

 李保银

 2019-03-11

 3,770

 阅读6分钟

 我们所在的内网环境需要部署一个类似CMS的应用，就是一些表格的CRUD，数据导出，人员权限管理等功能。想到Django做这方面的工作挺擅长的，而且开发量不大，于是选择Django作为开发基础。开发功能比较简单，差不多就是使用xadmin等插件实现以上功能。但有一个问题我们是不好绕过去的，那就是部署到一个内网环境，在内网pip等工具是不能使用的，但好在内网有一个yum服务器可以使用，所以我们决定在内网服务器上安装Docker，然后把开发环境的容器复制到生产环境实现部署。以下是主要的步骤：

安装开发环境的 Docker-ce

安装开发环境的 Docker-compose

配置开发环境

保存容器

安装生产环境的 Docker-ce 和 docker-compose

发送容器文件并运行

注意：我这里的开发环境是Ubuntu18.04，生产环境是Centos7.2。如果你是其他环境请自己检查差异，使用适合自己系统的命令。

安装开发环境的 Docker-ce

Docker 和 Docker-compose是我们这次部署需要重点演示的内容，Django 的应用部分我会尽量缩减的。Docker 负责容器虚拟化的底层部分，Docker-compos...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
