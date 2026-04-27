---
url: https://www.jianshu.com/p/d5e4f8061d6b
source: www.jianshu.com
date_saved: 2026-04-23
tags: DevOps, 设计, 学习-教程
content_type: article
word_count: 1195
status: 已处理
---
# 查看uwsgi进程和杀死进程 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/d5e4f8061d6b)

## 核心要点

1. 平台声明：文章内容（如有图片或视频亦包括在内）由作者上传并发布，文章内容仅代表作者本人观点，简书系信息发布平台，仅提供信息存储服务
2. 相关阅读更多精彩内容
MAC查看和杀死进程

R_X阅读 51,225评论 2赞 5

mac 查看端口和杀死进程
lsof -i tcp:port 查看3000端口 lsof -i tcp:3000 或查看进程 ps aux ..
3. dingfj阅读 1,654评论 0赞 3

Linux如何查看进程、杀死进程、启动进程等常用命令
转自CSDN，原文链接：http://blog.csdn.net/wojiaopanpan/article/det..

## 内容预览

查看uwsgi进程和杀死进程
#通过ps，查看uwsgi相关进程
ps aux|grep uwsgi
#kill pid会发送SIGTERM，只会导致重启，而不是结束掉。需要发送SIGINT或SIGQUIT，对应着是INT才可以
killall -s INT /usr/bin/uwsgi

最后编辑于 ：2019.05.16 11:23:01## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：