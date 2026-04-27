---
url: https://www.cnblogs.com/kingstrong/p/6027086.html
source: www.cnblogs.com
date_saved: 2026-04-23

content_type: article
word_count: 3321
status: 已处理
---

# shell-sed命令详解（转） - 小ks强 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/kingstrong/p/6027086.html)

## 核心要点

1. sed ‘$d’ file
删除最后一行的内容

sed ‘/abc/d’
删除包含abc的行

## 内容预览

小ks强

你只有足够强大，才能不顾别人的目光

博客园

首页

新随笔

联系

订阅

-->

管理

 shell-sed命令详解（转）

（转自http://blog.csdn.net/wl_fln/article/details/7281986）

Sed简介

sed是一种在线编辑器，它一次处理一行内容。处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，除非你使用重定向存储输出。Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等。以下介绍的是Gnu版本的Sed 3.02。

 

-i选项：直接作用源文件，源文件将被修改。

 

sed命令和选项：

 

a\
在当前行后添加一行或多行

c\
用新文本替换当前行中的文本

d
删除行

i\
在当前行之前插入文本

h
把模式空间的内容复制到暂存缓冲区

H
把模式空间的内容添加到缓冲区

g
取出暂存缓冲区的内容，将其复制到模式缓冲区

G
取出暂存缓冲区的内容，将其追加到模式缓冲区

l
列出非打印字符

p
打印行

n
读入下一行输入，并从下一条而不是第一条命令对其处理

q...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
