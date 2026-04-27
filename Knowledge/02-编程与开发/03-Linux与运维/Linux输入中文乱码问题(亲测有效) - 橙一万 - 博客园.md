---
url: https://www.cnblogs.com/orangebooks/p/12083509.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: DevOps
content_type: article
word_count: 964
status: 已处理
---

# Linux输入中文乱码问题(亲测有效) - 橙一万 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/orangebooks/p/12083509.html)

## 核心要点

1. 橙一万de博客

代码千万行，注释第一行,

命名不规范，同事两行泪

博客园

首页

新随笔

联系

订阅

-->

管理

 Linux输入中文乱码问题(亲测有效)

场景：当我们在Linux创建一个txt文件，输入中文的时候，发现输入的中文都是乱码

请看以下解决步骤:

1
2. 查看当前系统默认采用的字符集

 locale 查看当前系统默认采用的字符集

2
3. 查看系统当前编码

 echo $LANG 查看系统当前编码

如果输出为：
 en_US.UTF-8 英文
 zh_CN.UTF-8 中文

3

## 内容预览

橙一万de博客

代码千万行，注释第一行,

命名不规范，同事两行泪

博客园

首页

新随笔

联系

订阅

-->

管理

 Linux输入中文乱码问题(亲测有效)

场景：当我们在Linux创建一个txt文件，输入中文的时候，发现输入的中文都是乱码

请看以下解决步骤:

1. 查看当前系统默认采用的字符集

 locale 查看当前系统默认采用的字符集

2. 查看系统当前编码

 echo $LANG 查看系统当前编码

如果输出为：
 en_US.UTF-8 英文
 zh_CN.UTF-8 中文

3. 查看系统是否安装中文字符集

 locale -a |grep zh 查看系统是否安装中文字符集

如果出现了 zh 开头的，代表安装了中文字符集，直接进行第 4 步就行修改即可。

如果未出现 zh 开头的，则需要安装：
 yum -y groupinstall chinese-support 安装中文字符集
安装完成之后，修改系统字符集即可

4. 修改系统字符集

临时修改(当前终端生效)：
 export LANG="zh_CN.UTF-8"

永久修改：
 echo 'export LANG="zh_CN.UTF-8"' >> /etc/proflile 将单引号中的语句写入到 /etc/profile 文件
 source /etc/pro...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
