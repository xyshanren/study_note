---
url: https://www.jb51.net/LINUXjishu/43204.html
source: www.jb51.net
date_saved: 2026-04-23
tags: DevOps, 设计, 学习-教程
content_type: article
word_count: 1954
status: 已处理
---

# linux中查看各硬件资源信息的命令小结_LINUX_操作系统_脚本之家

> source: [www.jb51.net](https://www.jb51.net/LINUXjishu/43204.html)

## 核心要点

1. 查看Linux硬盘大小类型和硬
如何在 Linux 中查看 CPU 详细信息
2. 独树一帜的Arch Linux发行版分析
如何在Linux环境下制作 Win11装机U盘
3. 基于Rsync的强大Linux备份工具使用
Linux Kernel 6.13发布:附更新内容及新特性解读
五大特性引领创新

## 内容预览

脚本之家

 服务器常用软件

 手机版

 投稿中心

 关注微信

 快捷导航 

 网站首页

 网页制作

 网络编程

 脚本专栏

 脚本下载

 数据库

 服务器

 电子书籍

 操作系统

 网站运营

 平面设计

 其它

 媒体动画

 电脑基础

 硬件教程

 网络安全 

 您的位置：主页 > 操作系统 > LINUX > 

 linux中查看各硬件资源信息的命令小结

   发布时间：2012-04-15 17:18:06   作者：佚名   我要评论

 linux中查看各硬件资源信息的命令小结，需要的朋友可以参考下

 1.显卡信息
　　dmesg | grep -i vga 
　　lspci | grep -i vga //查看显卡信息

2.dmidecode | grep -i 'serrial number' //查看主板信息，查看主板的序列号

3.CPU信息

　　#通过/proc文件系统
　　cat /proc/cpuinfo
　　dmesg | grep -i cpu 
　　#通过查看开机信息
　　dmidecode -t processor

4.硬盘信息
　　fdisk -l //分区情况
　　df -h //大小情况
　　du -h //使用情况
　　dmesg | grep sd...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
