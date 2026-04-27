---
url: http://www.2cto.com/os/201112/114982.html
source: www.2cto.com
date_saved: 2026-04-23
tags: DevOps, 设计
content_type: article
word_count: 6000
status: 已处理
---

# linux tar打包解压详解 解压到指定文件夹 - Linux - 红黑联盟

> source: [www.2cto.com](http://www.2cto.com/os/201112/114982.html)

## 核心要点

1. linux tar打包解压详解 解压到指定文件夹

  

 编写shell脚本的时候经常需要解压缩到指定的文件夹，tar命令是最常用的

  

 参考一下说明，其中注意-C的用法
2.  

 tar命令

  

  

 解压文件到指定目录：tar -zxvf /home/zjx/aa.tar.gz -C /home/zjx/pf

  

 tar [-cxtzjvfpPN] 文件与目录...
3. 参数：

 -c ：建立一个压缩文件的参数指令(create 的意思)；

 -x ：解开一个压缩文件的参数指令

## 内容预览

linux tar打包解压详解 解压到指定文件夹

  

 编写shell脚本的时候经常需要解压缩到指定的文件夹，tar命令是最常用的

  

 参考一下说明，其中注意-C的用法。

  

 tar命令

  

  

 解压文件到指定目录：tar -zxvf /home/zjx/aa.tar.gz -C /home/zjx/pf

  

 tar [-cxtzjvfpPN] 文件与目录....

 参数：

 -c ：建立一个压缩文件的参数指令(create 的意思)；

 -x ：解开一个压缩文件的参数指令！

 -t ：查看tarfile 里面的文件！

 特别注意，在参数的下达中，c/x/t 仅能存在一个！不可同时存在！

 因为不可能同时压缩与解压缩。

 -z ：是否同时具有gzip 的属性？亦即是否需要用gzip 压缩？

 -j ：是否同时具有bzip2 的属性？亦即是否需要用bzip2 压缩？

 -v ：压缩的过程中显示文件！这个常用，但不建议用在背景执行过程！

 -f ：使用档名，请留意，在f 之后要立即接档名喔！不要再加参数！

 例如使用『tar -zcvfP tfile sfile』就是错误的写法，要写成

 『tar -zcvPf tfile sfile』才对喔！

 -p ：使用...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
