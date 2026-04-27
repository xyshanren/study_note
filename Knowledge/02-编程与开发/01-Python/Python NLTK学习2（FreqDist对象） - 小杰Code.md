---
url: http://www.coderjie.com/blog/e21021eec69411e6841d00163e0c0e36
source: www.coderjie.com
date_saved: 2026-04-23
tags: Python, 学习-教程
content_type: article
word_count: 2719
status: 已处理
---

# Python NLTK学习2（FreqDist对象） - 小杰Code

> source: [www.coderjie.com](http://www.coderjie.com/blog/e21021eec69411e6841d00163e0c0e36)

## 核心要点

1. introduced 6

 illustrates 1

 pepper 2

 independence 1

 desirous 1

 ..
2. FreqDist::plot(n)：该方法接受一个数字n，会绘制出现次数最多的前n项，在本例中即绘制高频词汇
3. fdist1.plot(10)

结果如下：

我们可以看到前10项的高频词，高频词大都是一些没意义词汇，如the、of、and等，他们大多是短单词

## 内容预览

Python NLTK学习2（FreqDist对象）

 发表于: 2016年12月20日  阅读: 32399

 除特别注明外，本站所有文章均为小杰Code原创

 转载请注明出处：
 http://www.coderjie.com/blog/e21021eec69411e6841d00163e0c0e36

 本系列博客为学习《用Python进行自然语言处理》一书的学习笔记。

频率分布

打开Python解释器，输入如下代码：

import nltk
from nltk.book import *
fdist1 = FreqDist(text1)

我们使用Text的实例对象作为参数生成了一个FreqDist对象，FreqDist继承自dict，所以我们可以像操作字典一样操作FreqDist对象。在本例中，FreqDist中的键为单词，值为单词的出现总次数。实际上FreqDist构造函数接受任意一个列表，它会将列表中的重复项给统计起来，在本例中我们传入的其实就是一个文本的单词列表。我们可以看看每个单词对应的出现次数：

for key in fdist1:
 print(key, fdist1[key])

结果如下：

 ...

 introduced 6

 illustrates 1

 pepper 2

 indepen...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
