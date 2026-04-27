---
url: https://www.jianshu.com/p/e7dcdcdeaa73
source: www.jianshu.com
date_saved: 2026-04-23
tags: Python, 后端, 学习-教程
content_type: article
word_count: 2023
status: 已处理
---
# Python-virtualenv创建虚拟环境 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/e7dcdcdeaa73)

## 核心要点

1. It is possible

that the option will be deprecated in a future version of virtualenv
2. 如果像停用虚拟环境

最后编辑于 ：2017.12.10 12:11:27## 内容预览

Python-virtualenv创建虚拟环境
Readthedocs-virtualenv

virtualenv.pypa

功能

virtualenv可以创建一个python虚拟环境，这个环境独立于系统原有的环境

Installation

$ sudo pip install virtualenv

或者通过apt-get 安装

$ sudo apt-get install virtualenv

Usage

新建目录my_project ,在目录内执行命令

$ virtualenv venv

这样就会生成my_project/venv 目录，里面有独立的python模块等

激活虚拟环境

$ source my_project/venv/bin/activate

激活后提示符会发生变化，默认情况下虚拟环境中是安装了pip的，使用pip安装模块时pip会将模块安装到venv 下

可以在虚拟环境下为所欲为 ，在虚拟环境中运行python程序与系统的python环境隔离，例如系统中装有requests 模块，而虚拟环境中没有安装requests ，那么在虚拟环境下尝试使用requests就会出现找不到模块的错误。

退出虚拟环境

使用命令deactivate 可以直接退出虚拟环境

参数

--python

指明环境中python的版本，例如

$ vir...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：