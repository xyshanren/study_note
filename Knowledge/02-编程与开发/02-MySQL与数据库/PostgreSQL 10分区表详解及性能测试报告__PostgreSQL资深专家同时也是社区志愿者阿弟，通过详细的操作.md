---
url: http://www.postgres.cn/news/viewone/1/271
source: www.postgres.cn
date_saved: 2026-04-23
tags: Stable-Diffusion, 后端, 数据库
content_type: article
word_count: 8000
status: 已处理
---

# PostgreSQL 10分区表详解及性能测试报告__PostgreSQL资深专家同时也是社区志愿者阿弟，通过详细的操作过程，带领大家领略一下即将发布的PostgreSQL10版本的一项重要改进－－内置分区表，让我们大家一起围观吧　！

__PostgreSQL中文社区: 世界上功能最强大的开源数据库....

> source: [www.postgres.cn](http://www.postgres.cn/news/viewone/1/271)

## 核心要点

1. 原作者：阿弟　　创作时间：2017-05-19 13:56:34+08
2. doudou586 发布于2017-05-19 13:56:34
3. 作者：PostgreSQL中文社区-- 阿弟 / 2017-5-18
4. 中国比较早的postgresql使用者,2001年就开始使用postgresql,自2003年底至2014年一直担任PGSQL中国社区论坛PostgreSQL的论坛板块版主、管理员，参与Postgresql讨论和发表专题文章7000多贴.
5. 拥有15年的erp设计,开发和实施经验,开源mrp系统PostMRP就是我的作品,该应用软件是一套基于Postgresql专业的制造业管理软件系统.
## 内容预览

PG中文社区 /

mdi-home

首页
社区新闻
中文文档
加入ACE

相关资料 mdi-chevron-down

{{ item.text }}

登录

1">

mdi-home
首页

mdi-chat-processing
社区新闻

mdi-book-open-variant
中文文档

mdi-account-multiple-check
加入ACE

mdi-file-multiple-outline
相关资料

mdi-blank
{{item.text}}

mdi-exit-to-app
退出账号

首页 -->
社区新闻 -->
强人随笔

PostgreSQL 10分区表详解及性能测试报告

原作者：阿弟　　创作时间：2017-05-19 13:56:34+08　　

doudou586 发布于2017-05-19 13:56:34　　

评论: 4　　
浏览: 30967　　
顶: 6818　
踩: 5682　

PostgreSQL 10分区表详解及性能测试报告

作者：PostgreSQL中文社区-- 阿弟 / 2017-5-18

欢迎大家踊跃投稿，投稿信箱：press@postgres.cn

作者简介：

中国比较早的postgresql使用者,2001年就开始使用postgresql,自2003年底至2014年一直担任PGSQL中国社区论坛PostgreSQL的论坛板块版主、管理员，参与Postgresql讨论和发表专题文章7000多贴.拥有15年的erp设计,开发和实施经验,开源mrp系统PostMRP就是我的作品,该应用软件是一套基于Postgresql专业的制造业管理软件系统.目前任职于--中国第一物流控股有限公司/运力宝（北京）科技有限公司,为公司的研发部经理

一、 测试环境

操作系统：CentOS 6.4
Postgresql版本号：10.0
CPU：Intel(R) Xeon(R) CPU E5-2407 v2 @ 2.40GHz 4核心 4线程
内存：32G
硬盘：2T SAS 7200

二、 编译安装PostgreSQL 10

－－编译安装及初始化

[root@ad source]# git clone git://git.postgresql.org/git/postgresql.git
[root@ad source]# cd postgresql
[root@ad source]# ./configure --prefix=/usr/local/pgsql10
[root@ad postgresql]# gmake -j 4
[root@ad postgresql]# gmake install
[root@ad postgresql]# su postgres
[postgres@ad postgresql]# /usr/local/pgsql10/bin/initdb --no-locale -E utf8 -D /home/postgres/data10/ -U postgres

－－修改一些参数

postgresql.conf

listen_addresses = '*'
port = 10000
shared_buffers = 8096MB
maintenance_work_mem = 512MB
effective_cache_size = 30GB
log_destination = 'csvlog'
logging_collector = o...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
