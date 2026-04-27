---
url: https://www.jianshu.com/p/b30d2765a091
source: www.jianshu.com
date_saved: 2026-04-23
tags: LLM, Python, DevOps
content_type: article
word_count: 2134
status: 已处理
---
# 如何在深度操作系统中安装Python3 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/b30d2765a091)

## 核心要点

1.## 内容预览

如何在深度操作系统中安装Python3
深度操作系统是linux系统在国内的一种表现，deepin中安装Python（系统中自带Python2）类似于Linux系统安装流程， 具体流程如下：
首先我们需要在Python官网下载我们所需要的Python版本,
网站（https://www.python.org/），在Download里选择linux操作系统版本进入下载界面

选择标记处文件进行下载
下载后一般为 python-3.7.1 tgz
相应的我们需要将下载的文件移到
sudo mv /home/user/python python-3.7.1 tgz
然后对文件进行解压
tar Jvxsf python-3.7.1 tgz
cd Python-3.7.1
之后进行配置
./configure --prefix=/usr/python --enable-shared CFLAGS=-fPIC
进行安装 
make && make install
查看是否安装成功
可进行环境配置，即软连
ln  /home/user/python/python3  /home/user/python
我自己在安装过程中出现了error（ModuleNotFoundError: No module named ‘_ctypes’）
可进行如下操作再进行make

sudo apt...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：