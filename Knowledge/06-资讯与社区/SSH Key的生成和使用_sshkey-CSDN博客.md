---
url: http://blog.csdn.net/yy243/article/details/53243948
source: blog.csdn.net
date_saved: 2026-04-23
tags: DevOps, 设计
content_type: article
word_count: 6000
status: 已处理
---

# SSH Key的生成和使用_sshkey-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/yy243/article/details/53243948)

## 核心要点

1. Enter file in which to save the key (/root/.ssh/id_rsa): 

Created directory ‘/root/.ssh’
2. Enter passphrase (empty for no passphrase): 

Enter same passphrase again: 

Your identification has been saved in /root/.ssh/id_rsa
3. Your public key has been saved in /root/.ssh/id_rsa.pub

## 内容预览

SSH Key的生成和使用

 最新推荐文章于 2026-03-18 00:47:50 发布

 转载

 最新推荐文章于 2026-03-18 00:47:50 发布
 ·
 10w+ 阅读

 ·

 15

 ·

 35

 本文介绍了 SSH key 的生成及使用方法，包括检查 ssh key 存在与否、生成 key、使用 ssh key 实现免密码登录等内容，并对 SSH 的基本结构和服务的启动与停止进行了概述。

SSH key生成及其使用 一、检查是否已经存在ssh key

通常sshkey会默认生成在用户家目录下所以查看家目录下是否存在.ssh 文件夹以及是否存在相关目录就行。~/.ssh/id_rsa

二、生成key

在控制台输入 

ssh-keygen -t rsa 

Note: -t 的意思是选择kye的type。分别有 RSA 和 DSA 两种。具体请自行百度 

控制台输出如下 

Generating public/private rsa key pair. 

Enter file in which to save the key (/root/.ssh/id_rsa): 

Created direc...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
