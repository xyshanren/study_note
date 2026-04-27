---
url: http://www.2cto.com/os/201207/140685.html
source: www.2cto.com
date_saved: 2026-04-23
tags: DevOps, 设计
content_type: article
word_count: 4697
status: 已处理
---

# linux crontab命令参数及用法详解--linux自动化定时任务cron - Linux - 红黑联盟

> source: [www.2cto.com](http://www.2cto.com/os/201207/140685.html)

## 核心要点

1. linux crontab命令参数及用法详解--linux自动化定时任务cron

 linux crontab命令参数及用法详解--linux自动化定时任务cron

 

crontab 命令

 

如果发现您的系统里没有这个命令,请安装下面两个软件包
2.  

45 4 1,10,22 * * /usr/local/etc/rc.d/lighttpd restart

上面的例子表示每月1、10、22日的4 : 45重启apache
3.  

10 1 * * 6,0 /usr/local/etc/rc.d/lighttpd restart

上面的例子表示每周六、周日的1 : 10重启apache

## 内容预览

linux crontab命令参数及用法详解--linux自动化定时任务cron

 linux crontab命令参数及用法详解--linux自动化定时任务cron

 

crontab 命令

 

如果发现您的系统里没有这个命令,请安装下面两个软件包.

  www.2cto.com  

vixie-cron

 

crontabs

 

crontab 是用来让使用者在固定时间或固定间隔执行程序之用，换句话说，也就是类似使用者的时程表。-u user 是指设定指定 user 的时程表，这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。 

 

常用参数:

 

crontab   -l   //查看当前用户下的cron任务

 

crontab -e  //编辑当前用户的定时任务

 

crontab -u  linuxso  -e  //编辑用户linuxso的定时任务

 

具体用法和格式:

 

基本格式 :

*　　*　　*　　*　　*　　command

分　时　日　月　周　命...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
