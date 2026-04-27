---
url: https://juejin.im/post/6844903608400478222
source: juejin.im
date_saved: 2026-04-23

content_type: article
word_count: 6000
status: 已处理
---

# Scrapy框架的使用之Scrapy通用爬虫通过Scrapy，我们可以轻松地完成一个站点爬虫的编写。但如果抓取的站点量非 - 掘金

> source: [juejin.im](https://juejin.im/post/6844903608400478222)

## 核心要点

1. Scrapy框架的使用之Scrapy通用爬虫

 崔庆才丨静觅

 2018-05-21

 9,248

 阅读21分钟

 通过Scrapy，我们可以轻松地完成一个站点爬虫的编写。但如果抓取的站点量非常大，比如爬取各大媒体的新闻信息，多个Spider则可能包含很多重复代码
2. 如果我们将各个站点的Spider的公共部分保留下来，不同的部分提取出来作为单独的配置，如爬取规则、页面解析方式等抽离出来做成一个配置文件，那么我们在新增一个爬虫的时候，只需要实现这些网站的爬取规则和提取规则即可
3. 一、CrawlSpider
在实现通用爬虫之前，我们需要先了解一下CrawlSpider，其官方文档链接为：http://scrapy.readthedocs.io/en/latest/topics/spiders.html#crawlspider

## 内容预览

Scrapy框架的使用之Scrapy通用爬虫

 崔庆才丨静觅

 2018-05-21

 9,248

 阅读21分钟

 通过Scrapy，我们可以轻松地完成一个站点爬虫的编写。但如果抓取的站点量非常大，比如爬取各大媒体的新闻信息，多个Spider则可能包含很多重复代码。

如果我们将各个站点的Spider的公共部分保留下来，不同的部分提取出来作为单独的配置，如爬取规则、页面解析方式等抽离出来做成一个配置文件，那么我们在新增一个爬虫的时候，只需要实现这些网站的爬取规则和提取规则即可。
本节我们就来探究一下Scrapy通用爬虫的实现方法。
一、CrawlSpider
在实现通用爬虫之前，我们需要先了解一下CrawlSpider，其官方文档链接为：http://scrapy.readthedocs.io/en/latest/topics/spiders.html#crawlspider。

CrawlSpider是Scrapy提供的一个通用Spider。在Spider里，我们可以指定一些爬取规则来实现页面的提取，这些爬取规则由一个专门的数据结构Rule表示。Rule里包含提取和跟进页面的配置，Spider会根据Rule来确定当前页面中的哪些链接需要继续爬取、哪些页面的爬取结果需要用哪个方法解析等。
CrawlSpider继承自Spider类。除了Spider类的所有方法和属性，它...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
