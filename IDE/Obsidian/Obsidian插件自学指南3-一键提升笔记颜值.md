---
title: Obsidian插件自学指南3-一键提升笔记颜值！
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649688526&idx=1&sn=6233a49037c360b910a13cbcaaef68c9&chksm=870ff7d9b0787ecf980bd5e4ef0b9fc05fd0a85e1f024049d2930ff358a0b6964bb962591a9a&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 今日目标：学习高亮块+浮动目录插件
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学习高亮块+浮动目录插件

Obsidian系列教程目录：

> 1- [[初识Obsidian]]
> 
> 2- [[从认识界面开始]]
> 
> 3- [[Obsidian笔记分类技巧]]
> 
> 4- [[Obsidian标签管理]]
> 
> 5- [[Obsidian插件自学指南1-编辑器篇]]
> 
> 6-[[Obsidian插件自学指南2-媒体篇]]


## 前面的文章讲过的插件

### 1- 编辑器插件

有提升文字编辑效率的插件Editing toolbar、编辑增强。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHAM0Kfm8Stw9bH4IuFCiaYbhoPkl0JiccPpcCZ9Tbc09Wu17QVSlFoOMA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 2- 媒体文件插件

有支持PPT、PDF、手绘思维导图等文件类型的插件，Marp、Annotator、Excalidraw、Mind Map等等。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHicf9mnYA2mR4qZ0qBIhY2UZwYH9dZ1EYuczvIJ288Y5LvHePibOVZicGQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


今天我们来聊聊，提升笔记颜值的美化插件。Admonition和Floating toc

## 高亮块

除了标题、段落、注释之外，最近流行了一种非常好看的笔记形式，叫做【高亮块】。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHa6ib8UyTxzB2c6vREXlFF8r7bPeicibtfxkfRm1v9nXwZl3tn4OFkVd1Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

淡淡的背景+ 可爱的图标 + 小标题，用在笔记里突出强调一些关键信息（比如文章出处等），非常的实用。

Notion、飞书、语雀等在线笔记软件，都已经跟进添加了高亮块的功能。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHXYECTKWHSI9pVfdUDtJXOl5duGPMagom3D3vq6z7AjlzkJjUiagopicg/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

用法非常简单，点击【+】按钮，或者按下【/】，然后选择【高亮块】，然后输入文字中就可以了。

## Admonition

Obsidian是Markdown格式的笔记，默认是不支持高亮块功能的，所以神通广大的开发者们，就开发了一个【高亮块】的插件【Admonition】。

在插件市场里面搜索Admonition并安装插件，就可以使用高亮块了。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHtfzCEAa0rGU1edsuKTMJdZVu6OgqKqZDLmnaUUVkS0lQ26TJB8uZsQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 添加高亮块

用法稍微有一些不同。按照下面的格式，输入Markdown就可以创建一个高亮块。

```
\`\`\`ad-note 标题笔记内容\`\`\`
```

得到的笔记效果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHQ1GlKE9U379ibdf6098NjXcqmyzKsfKWAmCs6DqTGib4ZhLfsh3juR5w/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

除了ad-note外，【Admonition】支持多种不同的关键字，实现不同颜色的高亮块。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHAnA4VTXhLyvdxA5bQSz0HRMJruthv2HD7caetwNiaiaX2jQ4zn6jay2Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 折叠高亮块

【Admonition】还支持可折叠的高亮块，只需要在标题后面，添加关键词collapse: open即可，效果如下：

```
\`\`\`ad-note 折叠的高亮块collapse: open加上collapse: open，就可以折叠高亮块了。\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNH6LZickGqVFZjD041GmrwNGoWYN0dWfAGgPkGn7tjzhAiaLVjic6GOV2fw/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## Callout

效果是不错，但是【Admonition】的使用起来感觉不太顺手，要打的字符太多，而且要保证\`\`\`成对儿出现。

最新版的Obsidian（笔者用的1.4.16版本），默认已经支持了高亮块，用法相对更简单一些。

```
> [!note] 标题> 正文
```

只需要输入>\[!\]就可以打开并选择高亮块。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHtZxxlwnR8zdSR86gGFpNr6nywWpRERUdYiaNEAkRVbqicGoCFCLvgzyg/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在高亮块后面加上+，就可以添加一个折叠的高亮块。

```
>[!note]+ 折叠的高亮块>这段文字是可以折叠的
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNH8nhalNm0QfNkw1aooDC0PQrXiaLdLo9asVZ9pE8u1pazH4TAGobG89A/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 高亮块快捷键

如果你还是觉得输入起来太麻烦，可以结合着【增强编辑】这个插件，使用快捷键添加高亮块。

输入高亮块的关键字，比如note，然后按下【Alt+;】一键转成高亮块。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHZibyCN1P9icDDQ8g2pU5rdZoeGh6SZPmmbXeNDcumt3JiaUy7icYmX5lPA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

输入note+按下【Alt+;】，可以转换成折叠的高亮块，非常简单高效。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHMz4oLE2ZoykywkQQibRx1afqqmTRwxza2RACx3wAs7j96FkicaxVfPFg/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 浮动目录

说完了高亮块，再来看【浮动目录】插件。

现在大部分的笔记软件都支持文章的大纲目录，只是在形式上会稍有不同。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHzkcHAIoPC3kA5XjFW7t3VooMgLpjLiaEe7AKLzxQw25xhdYpoiae488A/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这些目录的样式都是固定不变的，个别网站的大纲是一个**浮动的效果**，阅读的过程中，目录隐藏文字，不扰乱实现。

鼠标滑过大纲后自动显示文字，方便了解文章结构，这个动态效果的效果，简直泰裤辣！

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHXYjORiaadOeyGvas65vSUZUk3lGYqUKiaaylZmZvsHv3NjiadUZM8WqjA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

应该有很多人想要这个效果吧，所以就有开发者开发出了【Floating toc】插件，在Obsidian中完美的复刻出了上面的效果。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHmXY5QcgSiczrZqibPoWJvAkg8N1NXN12eDDeaMAaQ27ibaYdgQZAdQwkg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNHHI0fpJb0PGkhwsWLxdNia88UPkYaYW7BYSbpde5ctAZN0z2ylMNWXKw/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 小结

好了，以上就是今天的文章。很简单、很漂亮的两个插件。

> \- 高亮块：Admonition
> 
> \- 浮动目录：Floating toc

不知不觉研究了10几种插件了，一搞就是一上午，但是看一下自己写过的笔记，都是了了几个字。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SEJUjiaEcyaDzGWjM3ticbNH6R7Px35M1QcFBReTJE4OHfR65jG0YohcZy9DK7P8nINlp6v18Anh0Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

突然有点迷失的感觉，做笔记的目的到底是什么？这些插件真的很有必要吗？
