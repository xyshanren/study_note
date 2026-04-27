---
url: https://yococoxc.github.io/15445992804054.html
source: yococoxc.github.io
date_saved: 2026-04-23
tags: Python
content_type: article
word_count: 2222
status: 已处理
---

# Python Scrapy 图片下载 ImagesPipeline - 木子才的博客

> source: [yococoxc.github.io](https://yococoxc.github.io/15445992804054.html)

## 核心要点

1. 5.编辑 settings

import os

# 设置图片存储目录
IMAGES_STORE = os.path.join(os.path.dirname(os.path.dirname(__file__)),images)

R

## 内容预览

Python Scrapy 图片下载 ImagesPipeline

 参考

1.创建项目

scrapy startproject ImagesRename

2.编写 item

# -*- coding: utf-8 -*-

import scrapy

class ImagesrenameItem(scrapy.Item):
 imgurl = scrapy.Field()
 imgname = scrapy.Field()
 pass

3.spiders目录下创建蜘蛛文件：ImgsRename.py编写相应蜘蛛

# -*- coding: utf-8 -*-

import scrapy
from ImagesRename.items import ImagesrenameItem

class ImgsrenameSpider(scrapy.Spider):
 name = ImgsRename
 allowed_domains = [lab.scrapyd.cn]
 start_urls = [
 http://lab.scrapyd.cn/archives/55.html,
 http://lab.scrapyd.cn/archives/57.html,
 ]

 def par...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
