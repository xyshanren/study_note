---
url: https://www.linuxidc.com/Linux/2017-11/148214.htm
source: www.linuxidc.com
date_saved: 2026-04-23
tags: DevOps, 设计, 学习-教程
content_type: article
word_count: 5676
status: 已处理
---

# Linux ftp 安装及相关命令_服务器应用_Linux公社-Linux系统门户网站

> source: [www.linuxidc.com](https://www.linuxidc.com/Linux/2017-11/148214.htm)

## 核心要点

1. 安全性是编写VSFTP的初衷，除了这与生俱来的安全特性以外，高速与高稳定性也是VSFTP的两个重要特点
2. 在速度方面，使用ASCII代码的模式下载数据时，VSFTP的速度是Wu-FTP的两倍，如果Linux主机使用2.4.*的内核，在千兆以太网上的下载速度可达86MB/S
3. 在稳定方面，VSFTP就更加的出色，VSFTP在单机（非集群）上支持4000个以上的并发用户同时连接，根据Red Hat的Ftp服务器(ftp.redhat.com)的数据，VSFTP服务器可以支持15000个并发用户

## 内容预览

手机版

 你好，游客 登录

 注册

 搜索

 首页Linux新闻Linux教程数据库技术Linux编程服务器应用Linux安全Linux下载Linux主题Linux壁纸Linux软件数码手机电脑

 首页 → 服务器应用

 背景：

 阅读新闻

 Linux ftp 安装及相关命令

 [日期：2017-11-03]

 来源：Linux社区 

 作者：Linux

 [字体：大 中 小]

 1、VSFTP简介

　　VSFTP是一个基于GPL发布的类Unix系统上使用的FTP服务器软件，它的全称是Very Secure FTP 从此名称可以看出来，编制者的初衷是代码的安全。

　　安全性是编写VSFTP的初衷，除了这与生俱来的安全特性以外，高速与高稳定性也是VSFTP的两个重要特点。

　　在速度方面，使用ASCII代码的模式下载数据时，VSFTP的速度是Wu-FTP的两倍，如果Linux主机使用2.4.*的内核，在千兆以太网上的下载速度可达86MB/S。

　　在稳定方面，VSFTP就更加的出色，VSFTP在单机（非集群）上支持4000个以上的并发用户同时连接，根据Red Hat的Ftp服务器(ftp.redhat.com)的数据，VSFTP服务器可以支持15000个并发用户。

2、VSFTP安装及配置

    安...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
