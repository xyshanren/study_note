---
url: https://www.oschina.net/question/12_50469?sort=time
source: www.oschina.net
date_saved: 2026-04-23
tags: LLM, DevOps, 设计
content_type: article
word_count: 4856
status: 已处理
---

# 8 个实用的 Linux netcat 命令示例 - OSCHINA - 中文开源技术交流社区

> source: [www.oschina.net](https://www.oschina.net/question/12_50469?sort=time)

## 核心要点

1. Netcat 支持超时控制

多数情况我们不希望连接一直保持，那么我们可以使用 -w 参数来指定连接的空闲超时时间，该参数紧接一个数值，代表秒数，如果连接超过指定时间则连接会被终止
2. 服务器:

nc -l 2389 
客户端:

$ nc -w 10 localhost 2389 
该连接将在 10 秒后中断
3. 注意: 不要在服务器端同时使用 -w 和 -l 参数，因为 -w 参数将在服务器端无效果

## 内容预览

+

 首页
 开源软件
 问答
 博客
 翻译
 资讯
 Gitee
 众包
 活动
 专区
 源创会
 高手问答
 开源访谈
 周刊
 公司开源导航页

 登录
 注册

 资讯

 博客

 软件

 造物

 智库

 动弹

 专区

 活动

 工具

 培训

 Gitee

 新媒体

 OSC 直播栏目

 技术领航

 OSC 公众号

 硬核 + 嬉笑怒骂

 OSC 微博

 技术圈大 V 出没

 OSC 视频号

 AI 百科

 OSC 今日头条

 微头条显行业百态

 LFOSSA 公众号

 LF 开源软件学园

 模力方舟公众号

 大模型托管平台

 Gitee 服务号

 研发管理解决方案

 登录
 注册

  新版

 开源问答

 技术分享

 正文

 8 个实用的 Linux netcat 命令示例
 热

 红薯 发布于 2012/04/23 17:29

 阅读 33K+

 收藏 193

 评论 29

 Linux

 OSCHINA原创翻译

 Netcat 或者叫 nc 是 Linux 下的一个用于调试和检查网络工具包。可用于创建 TCP/IP 连接，最大的用途就是用来处理 TCP/UDP 套接字。

这里我们将通过一些实例来学习 netcat 命令。

1. 在服务...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
