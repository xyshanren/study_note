---
url: https://cuiqingcai.com/4421.html
source: cuiqingcai.com
date_saved: 2026-04-23
tags: Python, 学习-教程
content_type: article
word_count: 6000
status: 已处理
---

# 小白进阶之Scrapy第四篇（图片下载管道篇） | 静觅

> source: [cuiqingcai.com](https://cuiqingcai.com/4421.html)

## 核心要点

1. 0%

 PS： 爬虫不进入 img_url 函数的小伙伴儿 请尝试将将代码复制到你新建的 py 文件中
2. 2017/8/30 更新解决了网站防盗链导致下载图片失败的问题 这几天一直有小伙伴而给我吐槽说，由于妹子图站长把www.mzitu.com/all这个地址取消了。导致原来的那个采集爬虫不能用啦
3. 首先确保你的 Python 环境已安装 Scrapy!!!!!!!

## 内容预览

0%

 PS： 爬虫不进入 img_url 函数的小伙伴儿 请尝试将将代码复制到你新建的 py 文件中。 2017/8/30 更新解决了网站防盗链导致下载图片失败的问题 这几天一直有小伙伴而给我吐槽说，由于妹子图站长把www.mzitu.com/all这个地址取消了。导致原来的那个采集爬虫不能用啦。 正好也有小伙伴儿问 Scrapy 中的图片下载管道是怎么用的。 就凑合在一起把 mzitu.com 给重新写了一下。 首先确保你的 Python 环境已安装 Scrapy!!!!!!!! 命令行下进入你需要存放项目的目录并创建项目： 比如我放在了 D:\PycharmProjects

 1
2
3

 D:
cd PycharmProjects
scrapy startproject mzitu_scrapy

 我是 Windows！其余系统的伙伴儿自己看着办哈。 这都不会的小伙伴儿，快去洗洗睡吧。养足了精神从头看一遍教程哈！ 在 PyCharm 中打开我们的项目目录。 在 mzitu_scrapy 目录创建 run.py。写入以下内容：

 1
2

 from scrapy.cmdline import execute
execute(['scrapy', 'crawl', 'mzitu'])

 其中的 mzitu 就为待会儿 spider.py 文件中的 name 属性。...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
