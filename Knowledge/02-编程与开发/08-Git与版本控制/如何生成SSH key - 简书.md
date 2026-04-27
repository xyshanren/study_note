---
url: https://www.jianshu.com/p/31cbbbc5f9fa/
source: www.jianshu.com
date_saved: 2026-04-23
tags: DevOps
content_type: article
word_count: 6000
status: 已处理
---

# 如何生成SSH key - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/31cbbbc5f9fa/)

## 核心要点

1. 检查SSH keys是否存在

输入下面的命令，如果有文件id_rsa.pub 或 id_dsa.pub，则直接进入步骤3将SSH key添加到GitHub中，否则进入第二步生成SSH key

ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist

2
2. Your public key has been saved in /your_home_path/.ssh/id_rsa.pub
3. If you don't have `apt-get`, you might need to use another installer (like `yum`)

xclip -sel clip < ~/.ssh/id_rsa.pub
# Copies the co

## 内容预览

start_time: 2026-04-23 13:47:25 +0800

 如何生成SSH key

 Gevin

 IP属地: 安徽

 2.5

 2014.08.28 17:01

 字数 394

 如何生成SSH key

本文载于Gevin的博客

原文地址：http://blog.igevin.info/posts/generate-ssh-key-for-git/

SSH key提供了一种与GitHub通信的方式，通过这种方式，能够在不输入密码的情况下，将GitHub作为自己的remote端服务器，进行版本控制

步骤

检查SSH keys是否存在

生成新的ssh key

将ssh key添加到GitHub中

如何生成SSH KEY

1. 检查SSH keys是否存在

输入下面的命令，如果有文件id_rsa.pub 或 id_dsa.pub，则直接进入步骤3将SSH key添加到GitHub中，否则进入第二步生成SSH key

ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist

2. 生成新的ssh key

第一步：生成public/private rsa key pair

在命令行中输入ssh-keygen -t rsa -C "your_email...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
