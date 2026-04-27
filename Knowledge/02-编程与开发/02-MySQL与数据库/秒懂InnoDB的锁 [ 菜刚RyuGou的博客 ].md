---
url: https://i6448038.github.io/2019/02/23/mysql-lock/
source: i6448038.github.io
date_saved: 2026-04-23
tags: 后端, 数据库, 算法
content_type: article
word_count: 3332
status: 已处理
---

# 秒懂InnoDB的锁 [ 菜刚RyuGou的博客 ]

> source: [i6448038.github.io](https://i6448038.github.io/2019/02/23/mysql-lock/)

## 核心要点

1. RyuGous blog

 首页

 关于

 首页

 /

 关于

 /

-->

 秒懂InnoDB的锁

 今天我们来聊聊MySQL中InnoDB存储引擎的锁
2. lock和 latch
latch
latch在MySQL中是用来保证并发多线程操作操作临界资源的锁，锁定的对象线程，是和咱们使用的Java等传统语言中的锁意义相近，而且没有死锁检测的机制
3. lock
lock是MySQL中在事务中使用的锁，锁定的对象是事务，来锁定数据库中表、页、行；通常只有在事务commit或者rollback后进行释放。lock是有死锁机制的，当出现死锁时，lock有死锁机制来解决死锁问题：超时时间(参数innodb_lock_wait_timeout)、wait-for graph

## 内容预览

RyuGous blog

 首页

 关于

 首页

 /

 关于

 /

-->

 秒懂InnoDB的锁

 今天我们来聊聊MySQL中InnoDB存储引擎的锁。

锁是数据库系统系统区别于文件系统的一个关键特性。

lock和 latch
latch
latch在MySQL中是用来保证并发多线程操作操作临界资源的锁，锁定的对象线程，是和咱们使用的Java等传统语言中的锁意义相近，而且没有死锁检测的机制。

lock
lock是MySQL中在事务中使用的锁，锁定的对象是事务，来锁定数据库中表、页、行；通常只有在事务commit或者rollback后进行释放。lock是有死锁机制的，当出现死锁时，lock有死锁机制来解决死锁问题：超时时间(参数innodb_lock_wait_timeout)、wait-for graph。

我们通常讲的MySQL的“锁”，一般就是说的lock。

以下就是InnoDB中“锁”的大分类：

lock的种类
MySQL Lock大体上可以分为：表锁、行锁、意向锁三种。

共享排他锁
行锁分为：S Lock和X Lock。S Lock ：读锁；X Lock：写锁。
两锁之间的兼容性如下。

1
2
3
4
 X S
X 不兼容 不兼容
S 不兼容 兼容

简单总结为：读锁可以读，读锁不可写；写锁不可读也tm不可写。

...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
