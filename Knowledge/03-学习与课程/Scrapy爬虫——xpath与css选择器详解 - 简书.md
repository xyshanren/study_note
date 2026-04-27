---
url: https://www.jianshu.com/p/489c5d21cdc7
source: www.jianshu.com
date_saved: 2026-04-23
tags: 学习-教程
content_type: article
word_count: 5985
status: 已处理
---

# Scrapy爬虫——xpath与css选择器详解 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/489c5d21cdc7)

## 核心要点

1. Scrapy爬虫——xpath与css选择器详解

有条件的请支持慕课实战正版课程，本blog仅仅是归纳总结，自用
2. 一、xpath部分

1.1 xpath简介

xpath简介.png

1.2 xpath语法

子元素：仅仅指节点下面一层的元素

后代元素：指标签下面任意层级的元素

父元素、祖先（先辈）元素同理
3. xpath语法图

1.3 xpath谓语语法

谓语（Predicates）谓语用来查找某个特定的节点或者包含某个指定的值的节点。谓语被嵌在方括号中

## 内容预览

Scrapy爬虫——xpath与css选择器详解

有条件的请支持慕课实战正版课程，本blog仅仅是归纳总结，自用。

一、xpath部分

1.1 xpath简介

xpath简介.png

1.2 xpath语法

子元素：仅仅指节点下面一层的元素

后代元素：指标签下面任意层级的元素

父元素、祖先（先辈）元素同理。

xpath语法图

1.3 xpath谓语语法

谓语（Predicates）谓语用来查找某个特定的节点或者包含某个指定的值的节点。谓语被嵌在方括号中。

xpathwei'yu

1.4 xpath其他语法

通配符
描述

*
匹配任何元素节点。

@*
匹配任何属性节点。

node()
匹配任何类型的节点。

xpath其他语法

二、css选择器

css选择器

css选择器2

css选择器3

三、scrapy选择器实战

Scrapy选择器构建于 lxml 库之上，这意味着它们在速度和解析准确性上非常相似。

我们将使用 Scrapy shell

(提供交互测试)和位于Scrapy文档服务器的一个样例页面，来解释如何使用选择器：

http://doc.scrapy.org/en/latest/_static/selectors-sample1.html

这里是它的HTML源码:

<html>
 <head>
 ...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
