---
url: https://cloud.tencent.com/developer/article/1445263
source: cloud.tencent.com
date_saved: 2026-04-23
tags: LLM, 前端, 后端
content_type: article
word_count: 6000
status: 已处理
---

# Scrapy框架中crawlSpider的使用——爬取内容写进MySQL和拉勾网案例-腾讯云开发者社区-腾讯云

> source: [cloud.tencent.com](https://cloud.tencent.com/developer/article/1445263)

## 核心要点

1. CrawlSpider是Spider的派生类，Spider类的设计原则是只爬取start_url列表中的网页，而CrawlSpider类定义了一些规则(rule)来提供跟进link的方便的机制，从爬取的网页中获取link并继续爬取
2. callback： 从link_extractor中每获取到链接时，参数所指定的值作为回调函数，该回调函数接受一个response作为其第一个参数
3. 注意：当编写爬虫规则时，避免使用parse作为回调函数。由于CrawlSpider使用parse方法来实现其逻辑，如果覆盖了 parse方法，crawl spider将会运行失败

## 内容预览

菲宇

Scrapy框架中crawlSpider的使用——爬取内容写进MySQL和拉勾网案例

关注作者

腾讯云
开发者社区

文档建议反馈控制台登录/注册

首页学习
活动
专区
圈层
工具

MCP广场

文章/答案/技术大牛搜索
搜索关闭

发布

菲宇

社区首页 >专栏 >Scrapy框架中crawlSpider的使用——爬取内容写进MySQL和拉勾网案例

Scrapy框架中crawlSpider的使用——爬取内容写进MySQL和拉勾网案例

菲宇

关注

发布于 2019-06-13 11:39:33
发布于 2019-06-13 11:39:33
1.5K0

举报

文章被收录于专栏：菲宇菲宇

Scrapy框架中分两类爬虫，Spider类和CrawlSpider类。该案例采用的是CrawlSpider类实现爬虫进行全站抓取。
CrawlSpider是Spider的派生类，Spider类的设计原则是只爬取start_url列表中的网页，而CrawlSpider类定义了一些规则(rule)来提供跟进link的方便的机制，从爬取的网页中获取link并继续爬取。
创建CrawlSpider模板：
代码语言：javascript

复制

scrapy genspider -t crawl spider名称 www.xxxx.com

LinkExtrac...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
