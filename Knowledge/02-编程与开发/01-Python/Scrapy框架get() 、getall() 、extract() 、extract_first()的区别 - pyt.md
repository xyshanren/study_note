---
url: https://segmentfault.com/a/1190000018559454
source: segmentfault.com
date_saved: 2026-04-23
tags: Python, 后端
content_type: article
word_count: 4441
status: 已处理
---

# Scrapy框架get() 、getall() 、extract() 、extract_first()的区别 - python后端实战经验分享 - SegmentFault 思否

> source: [segmentfault.com](https://segmentfault.com/a/1190000018559454)

## 核心要点

1. Scrapy框架get() 、getall() 、extract() 、extract_first()的区别

rabbitcoder

2019-03-18 阅读 4 分钟

1

开篇明义：get() 、getall() 是新版本的方法，extract() 、extract_first()是旧版本的方法
2. 前者更好用，取不到就返回None，后者取不到就raise一个错误
3. 推荐使用新方法，官方文档中也都改用前者了

看官方文档（链接附在文末），看到了关于get()、get()方法的使用，查阅网络没有资料，那就自己记录一下

## 内容预览

Scrapy框架get() 、getall() 、extract() 、extract_first()的区别

rabbitcoder

2019-03-18 阅读 4 分钟

1

开篇明义：get() 、getall() 是新版本的方法，extract() 、extract_first()是旧版本的方法。

前者更好用，取不到就返回None，后者取不到就raise一个错误。

推荐使用新方法，官方文档中也都改用前者了

看官方文档（链接附在文末），看到了关于get()、get()方法的使用，查阅网络没有资料，那就自己记录一下。
y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~y( ˙ᴗ. )耶~
先说结论：
对于scrapy.selector.unified.SelectorList对象，getall()==extract(),get()==extract_first()
对于scrapy.selector.unified.Selector对象，getall()==extract(),get()!=extract_first()
使用scrapy shell 进行测试

scrapy shell https://gavbus668.com/
得到如下结果：

皆是常规...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
