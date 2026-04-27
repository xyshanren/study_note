---
url: https://www.jianshu.com/p/2857638f039d
source: www.jianshu.com
date_saved: 2026-04-23
tags: LLM, Python, DevOps
content_type: article
word_count: 5415
status: 已处理
---

# virtualenv 虚拟环境安装 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/2857638f039d)

## 核心要点

1. 它可以用来解决Python项目开发和运行过程中的依赖项和版本问题，而不必和其他项目的Python环境以及全局的Python环境发生冲突
2. ENV/lib 和 ENV/include 目录中包含了虚拟环境ENV使用的库文件。在虚拟环境中安装的第三方包会安装在 ENV/lib/python3.4/site-packages 目录下
3. ENV/bin 目录里面放置了可执行文件，在里面有新安装的Python 包中的可执行程序，包括pip等相关工具

## 内容预览

virtualenv 虚拟环境安装
一、virtualenv 虚拟环境安装

   virtualenv 工具可以用来在 Linux 操作系统中创建一个虚拟的Python环境。这个环境是独立的、隔离的，拥有自己的环境安装目录（而不是把所有的第三方包等都安装在 /usr/lib/python3.4/site-packages 目录（Python3.4的默认Linux安装目录）下）。

   它可以用来解决Python项目开发和运行过程中的依赖项和版本问题，而不必和其他项目的Python环境以及全局的Python环境发生冲突。

1、安装 virtualenv

建议使用 pip 工具安装 virtualenv 包：

$ pip install virtualenv

2、创建虚拟环境

安装完成后，可以使用 virtualenv 命令创建放置虚拟环境的目录：

$ virtualenv [OPTIONS] [虚拟环境名称]

创建原理：

   在目录 ENV 里会初始化虚拟环境的相关目录和文件，包括 Python 语言本身的环境以及 pip 等相关程序都会在这个目录里创建（拷贝）一份新的。

ENV/lib 和 ENV/include 目录中包含了虚拟环境ENV使用的库文件。在虚拟环境中安装的第三方包会安装在 ENV/lib/python3.4/site-packages 目录下...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
