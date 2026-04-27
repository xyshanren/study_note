---
url: https://jingyan.baidu.com/article/a378c960ec5b1cb328283092.html
source: jingyan.baidu.com
date_saved: 2026-04-23
tags: Python, 数据库, DevOps
content_type: article
word_count: 1404
status: 已处理
---

# Linux下安装Anaconda后如何授权普通用户使用-百度经验

> source: [jingyan.baidu.com](https://jingyan.baidu.com/article/a378c960ec5b1cb328283092.html)

## 核心要点

1. [yes|no]
[no] >>> yes
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/ai/.bashrc
2. [yes|no]
[no] >>> yes
Do you wish to proceed with the installation of Microsoft VSCode
3. [yes|no]
>>> no

4
验证Anaconda是否安装成功：
$ cd
$

## 内容预览

百度经验 > 游戏/数码 > 电脑 > 电脑软件

Linux下安装Anaconda后如何授权普通用户使用

原创
|
浏览：5424
|
更新：2018-03-05 11:18

用root安装Anaconda后发现普通用户没有权限，各种修改权限直至崩溃，搜了一圈最后发现，正确答案是“不要用root安装！！！”
官网的安装手册里写You do NOT need root privileges to install Anaconda，其实根本不应该使用root权限（或者sudo）。
下面是安装步骤

工具/原料
Linux

方法/步骤
1
将安装包上传到服务器的/tmp目录（可以用共享文件夹或者FTP或者干脆在Linux里用火狐来下载）

2
用普通用户登录终端

3
$ cd /tmp
$ bash Anaconda3-5.1.0-Linux-x86_64.sh
注意下面2处不要回车用缺省的选项：
Do you accept the license terms? [yes|no]
[no] >>> yes
Do you wish the installer to prepend the Anaconda3 install location
to PATH in your /home/...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
