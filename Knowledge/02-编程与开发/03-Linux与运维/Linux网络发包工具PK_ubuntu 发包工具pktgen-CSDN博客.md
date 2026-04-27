---
url: https://blog.csdn.net/nuoline/article/details/8610597
source: blog.csdn.net
date_saved: 2026-04-23
tags: DevOps, 设计
content_type: article
word_count: 6000
status: 已处理
---

# Linux网络发包工具PK_ubuntu 发包工具pktgen-CSDN博客

> source: [blog.csdn.net](https://blog.csdn.net/nuoline/article/details/8610597)

## 核心要点

1. How to
use the new enhancements

III
2. What i
have change in the code

First of all I want to mention that this patch was only tested on a
x86

PC with a v2.6.8 Linux Kernel
3. But please report problems to
me:

fabian_at_net.in.tum.de (substitute _at_ with )

I

## 内容预览

Linux网络发包工具PK

 最新推荐文章于 2022-12-03 22:01:01 发布

 原创

 最新推荐文章于 2022-12-03 22:01:01 发布
 ·
 2.7k 阅读

 ·

 0

 ·

 1

 ·

 CC 4.0 BY-SA版权

 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

 Linux服务器
 专栏收录该内容

 42 篇文章

 订阅专栏

如果想做模仿网络攻击的测试选择高速小包发送工具最好还是可以指定协议的。当然我们研究这些可不是打算用来攻击他人的机器搞网络破坏的而是用来通过该方法测试收数据体验一下被攻击的感觉哈哈也顺便衡量一下机器的性能。这方面smartbit测试仪可以完全可以满足。可惜啊一台都得好几十万对于大多数人来说都不太划算。那么还有没有软件的发包工具可以实现高速按指定协议发送数据包啊有。还是要归功于linux的开源精神的许多网络黑客的无私奉献。我们可以采用linux内核自带的发包工具pktgen或者经常被用来进行网络攻击的stream源...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
