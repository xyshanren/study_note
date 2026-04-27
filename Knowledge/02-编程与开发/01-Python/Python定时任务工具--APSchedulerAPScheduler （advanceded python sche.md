---
url: https://juejin.im/post/5d3718c35188251b2569f9e8#heading-15
source: juejin.im
date_saved: 2026-04-23
tags: Python, DevOps
content_type: article
word_count: 6000
status: 已处理
---

# Python定时任务工具--APSchedulerAPScheduler （advanceded python sche - 掘金

> source: [juejin.im](https://juejin.im/post/5d3718c35188251b2569f9e8#heading-15)

## 核心要点

1. Python定时任务工具--APScheduler

 Simon_Zhou

 2019-07-23

 12,582

 阅读6分钟

 APScheduler （advanceded python scheduler）是一款Python开发的定时任务工具
2. from apscheduler.schedulers.background import BackgroundScheduler

scheduler = BackgroundScheduler()
scheduler.start() # 此处程序不会发生阻塞
AsyncIOScheduler : 当你的程序使用了asyncio的时候使用
3. GeventScheduler : 当你的程序使用了gevent的时候使用

## 内容预览

Python定时任务工具--APScheduler

 Simon_Zhou

 2019-07-23

 12,582

 阅读6分钟

 APScheduler （advanceded python scheduler）是一款Python开发的定时任务工具。
文档地址 apscheduler.readthedocs.io/en/latest/u…
特点：
不依赖于Linux系统的crontab系统定时，独立运行

可以动态添加新的定时任务，如下单后30分钟内必须支付，否则取消订单，就可以借助此工具（每下一单就要添加此订单的定时任务）

对添加的定时任务可以做持久保存

1 安装
pip install apscheduler2 组成
APScheduler 由以下四部分组成：
triggers 触发器 指定定时任务执行的时机

job stores 存储器 可以将定时持久存储

executors 执行器 在定时任务该执行时，以进程或线程方式执行任务

schedulers 调度器 常用的有BackgroundScheduler（后台运行）和BlockingScheduler(阻塞式)

3 使用方式
from apscheduler.schedulers.background import BlockingScheduler

# 创建定时任务的调度器对象
scheduler...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
