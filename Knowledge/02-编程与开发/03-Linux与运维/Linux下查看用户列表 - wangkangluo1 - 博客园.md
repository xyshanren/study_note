---
url: https://www.cnblogs.com/wangkangluo1/archive/2011/09/23/2185977.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: DevOps
content_type: article
word_count: 554
status: 已处理
---

# Linux下查看用户列表 - wangkangluo1 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/wangkangluo1/archive/2011/09/23/2185977.html)

## 核心要点

1. 俺的centos vps上面不知道添加了多少个账户，今天想清理一下，但是以前还未查看过linux用户列表，google了一下，找到方便的放：
2. cat /etc/passwd|grep -v nologin|grep -v halt|grep -v shutdown|awk -F":" '{ print $1"|"$3"|"$4 }'|more
## 内容预览

wangkangluo1

博客园

首页

新随笔

联系

管理

订阅

Linux下查看用户列表

原文地址：http://xiaod.in/read.php?77

俺的centos vps上面不知道添加了多少个账户，今天想清理一下，但是以前还未查看过linux用户列表，google了一下，找到方便的放：
一般情况下是

cat /etc/passwd 可以查看所有用户的列表
w 可以查看当前活跃的用户列表
cat /etc/group 查看用户组

但是这样出来的结果一大堆，看起来嘿负责，于是继续google
找到个简明的layout命令

cat /etc/passwd|grep -v nologin|grep -v halt|grep -v shutdown|awk -F":" '{ print $1"|"$3"|"$4 }'|more

这样一来，show出来的就只是用户列表和一点点东西了~~~~

完

posted @
2011-09-23 11:22
wangkangluo1
阅读(183279)
评论(2)

收藏
举报

刷新页面返回顶部

博客园
&copy; 2004-2026

浙公网安备 33010602011771号
浙ICP备2021040463号-3
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
