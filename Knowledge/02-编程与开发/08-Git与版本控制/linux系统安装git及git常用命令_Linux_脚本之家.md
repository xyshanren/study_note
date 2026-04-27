---
url: https://www.jb51.net/article/46295.htm
source: www.jb51.net
date_saved: 2026-04-23
tags: DevOps, 设计, 学习-教程
content_type: article
word_count: 3273
status: 已处理
---

# linux系统安装git及git常用命令_Linux_脚本之家

> source: [www.jb51.net](https://www.jb51.net/article/46295.htm)

## 核心要点

1. 这篇文章主要介绍了linux系统安装git及git常用命令,大家参考使用吧
2. $ sudo aptitude install git-doc git-svn git-email git-gui gitk
3. git软件包包含了大部分Git命令，是必装的软件包，第二行命令也是Git软件包，但是是单独发布的，可以选择安装。
4. $ git clone git://远程Git库地址 filename
5. filename 是你本地的文件夹名字将远程库克隆到这个文件夹，此文件是自己建立的
## 内容预览

脚本之家

服务器常用软件

手机版

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

星外虚拟主机

华众虚拟主机

Linux

win服务器

FTP服务器

DNS服务器

Tomcat

nginx

zabbix

云和虚拟化

服务器其它

您的位置：首页 → 网站技巧 → 服务器 → Linux → git下载远程项目

linux系统安装git及git常用命令

更新时间：2014年01月27日 14:48:33 作者：

这篇文章主要介绍了linux系统安装git及git常用命令,大家参考使用吧

1 安装GIT
复制代码 代码如下:

$ sudo aptitude install git
$ sudo aptitude install git-doc git-svn git-email git-gui gitk

git软件包包含了大部分Git命令，是必装的软件包，第二行命令也是Git软件包，但是是单独发布的，可以选择安装。

2 下载远程项目的GIT库到本地
[code]
$ git clone git://远程Git库地址 filename
[code]

filename 是你本地的文件夹名字将远程库克隆到这个文件夹，此文件是自己建立的

3 常用命令

（1）git branch 　查看本地分支

（2）git branch -a 　　　　 查看远程分支

（3）git checkout branchname 切换分支　

（4）git add　yourfile　　　　

（5）git commit -a -m "描述"　　 提交你当前开发到暂存区，可以理解为你本地的GIT库

（6）git pull　 更新，如果几个人同时在一个分枝上开发，可能会造成不同步，造成自己本地的GIT库落后或提前远程GIT库，这时候就要更新自己本地的库。

（7）git push　提交，将自己开发的代码提交到对应的远程分之上去

（8）git status 查看工作区状态，及查看在此分支上进行了那些操作

（9）git log　　查看操作日志，还是挺有用的

（10）git merge 合并分支，自己开发的模块最终要合并到项目的总分枝上去，这是要先切换到项目总分支，然后 git merge 自己的分支

（11）git branch -d/D yourbranch 删除本地分支

（12）git push origin :yourbranch 删除远程分支

您可能感兴趣的文章:如何在Linux 系统中使用apt 包管理器安装 Git LFS
Linux/Ubuntu Git从安装到使用的方法步骤
linux安装git的方法步骤
在Ubuntu Linux上安装和使用Git和GitHub
Linux 和Windows 安装Git 步骤详细介绍
Linux安装Git详细图文教程及遇到坑

linux

git

相关文章

linux下yum安装时出现Loaded plugins: fastestmirror的解决办法
这篇文章主要给大家介绍了linux下yum安装时出现Loaded plugins: fastestmirror,使用 yum 出现 Loaded plugins: fastestmirror,文中有详细的解决方法,通过代码介绍的非常详细,需要的朋友可以参...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
