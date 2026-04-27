---
url: https://www.xttblog.com/?p=2115
source: www.xttblog.com
date_saved: 2026-04-23
tags: 前端, 后端, 数据库
content_type: homepage
word_count: 1706
status: 已处理
---

# PostgreSQL数据库查看用户权限  业余草

> source: [www.xttblog.com](https://www.xttblog.com/?p=2115)

## 核心要点

1. -->-->
最后，欢迎关注我的个人微信公众号：业余草（yyucao）！可加作者微信号：xttblog2。备注：“1”，添加博主微信拉你进微信群。备注错误不会同意好友申请。再次感谢您的关注！后续有精彩内容会第一时间发给您！原创文章投稿请发送至532009913@qq.com邮箱。商务合作也可添加作者微信进行联系

## 内容预览

业余草

 首页

HTML5

JAVA

SQL

业余杂谈

NDIS

敏捷开发

公众号精选

 订阅

 -->

 你的位置：业余草 > SQL > PostgreSQL数据库查看用户权限

 公告：“业余草”微信公众号提供免费CSDN下载服务(只下Java资源)，关注业余草微信公众号，添加作者微信：xttblog2，发送下载链接帮助你免费下载！

 本博客日IP超过2000，PV 3000 左右，急需赞助商。

极客时间所有课程通过我的二维码购买后返现24元微信红包，请加博主新的微信号：xttblog2，之前的微信号好友位已满，备注：返现

受密码保护的文章请关注“业余草”公众号，回复关键字“0”获得密码

所有面试题(java、前端、数据库、springboot等)一网打尽，请关注文末小程序

【腾讯云】1核2G5M轻量应用服务器50元首年，高性价比，助您轻松上云

 PostgreSQL 数据库国内的资料真的是少，最近想查询一下其相关用户的数据库权限时，网上给的答案都是答非所问啊。很着急，下面总结一下权限相关的语句，以备以后使用。

 查看某用户的表权限

select * from information_schema.table_privileges where grantee=xttblog;

 查看usage权限表

s...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
