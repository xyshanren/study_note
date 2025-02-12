---
title: 效率翻倍：掌握Dataview查询条件，轻松管理您的知识库
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649689438&idx=1&sn=7f2a32d5c5c0c5a6cff7c6984c8df95d&chksm=870ff349b0787a5f321c38868e772f36f4fb4dd043c4ddb5cdcdb34531ad32a5713931e19d2c&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 今日目标：学会使用where查询
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学会使用where查询

Obsidian系列文章目录：

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
> 6- [[Obsidian插件自学指南2-媒体篇]]
>
> 7- [[Obsidian插件自学指南3-一键提升笔记颜值]]
>
> 8- [[Obsidian一键自动创建笔记，就用这个插件]]
>
> 9- [[Obsidian一键创建日报，一定要学会【模版】插件]]
>
>10- [[Obsidian效率翻倍的模版插件，Templater]]
>
>11- [[Obsidian高效编辑笔记第3弹，Quick Add插件]]
>
>12- [[7天研究3个插件，我悟了，笔记原来要这样做]]
>
>13- [[Obsidian插件指南：让日报与周报管理变得简单又高效]]
>
>14- [[Obsidian日程管理插件，Day planner使用全攻略]]
>
>15- [[Obsidian插件Tasks：你的时间管理利器，让每一天都井然有序]]
>
>16- [[Obsidian任务可视化管理插件，Kanban]]
>
>17- [[Obsidian插件，如何利用Recent Files提升笔记使用效率]]
>
>18- [[Obsidian最强插件Dataview，基础入门讲解]]
>
>19- [[精准管理你的知识库：Dataview插件的高级查询技巧]]

**《从零开始学习Obsidian》系列教程，今天是学习OB的第18天！

---
## 回顾

上一篇文章，我们学习了使用from来设置dataview查询笔记的来源。比如：

- 查询指定【文件夹】中的笔记
- 查询指定【标签】下的笔记
- 查询笔记中的【链接】等等

虽然查询的范围缩小了，但依然无法实现精确的笔记查询，比如：

- 按照关键词搜索笔记。
- 按照时间，查询最近一周的笔记。
- 按照备注，查询相关的笔记等等。

今天的文章，我们来学习dataview中的where关键字，实现上面这些精准的笔记查找。

## 理解where

where是dataview查询中的条件关键字，类似Excel中的IF函数，我们可以通过逻辑判断，在where后面添加各种查询条件。

比如，查询2024/4/1之后创建的笔记，就可以这样写dataview查询语句。

```
\`\`\`dataviewlistwhere file.ctime>date("2024-04-01")\`\`\`
```

查询出来的笔记就少了很多。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwrWMg8GZU3DiaUwKFYpMAjcaplRUuouGVudQBFk2ibM1e2TjAnSwQo35g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

where后面可以进行的逻辑片段有很多，比如：

```
=：表示等于>：表示大于<：表示小于>=：表示大于等于<=：表示小于等于!：表示不等于contains：表示是否包含
```

接下来，我们通过几个案例，来学习where的实战用法。

## 实战案例

### 按文件名搜索

#### ◆文件名称

查询文件名称中，包含关键字“笔记”的笔记。查询代码如下：

```
\`\`\`dataviewtablewhere contains(file.name,"笔记")\`\`\`
```

查询结果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwKWQPicKP5VBB3ndeMbdUt8OGQ2KicfSgQ3ebfmGfIMNKgkaJjMf9IUMg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

contains是dataview中的函数，用来判断文本中是否包含指定的字符。包含2个参数：

- 参数1：要查询的原始文本。比如上面代码中的file.name。
- 参数2：要查询的关键字。比如上面的“笔记”。

如果找到了关键字就返回true，否则返回false。

### ◆标签

相同的用法，也可以用在标签上，下面的语句用来查询标签中包含“标签”两个字的笔记。

```
\`\`\`dataviewtablewhere contains(file.tags,"标签")\`\`\`
```

更多file相关的属性如下，都可以用到where中进行条件判断。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOw9TOdbkXQyg1AeTMfia057TQFQPqRDDLENqv3AX0bHxQeEulRmInRibQw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 按时间搜索

如果笔记数量比较多，一定要养成定期回看的习惯，否则笔记很容易收藏吃灰。所以，按照时间搜索过去某个时段的笔记，是我日常的高频需求。

### ◆最近一周

比如，想查看最近一周创建的所有笔记，就可以使用下面的代码：

```
\`\`\`dataviewtablewhere file.ctime >= date(today) - dur(7 day)\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwzdt7FlsHnqc6LsxZKCEKgp1QZLZ70HWyzibAHkUeALomSYwComrm5dA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### ◆本月

查询最近一个月创建的笔记，代码如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwNTqjo3BiaqlHicplg56HnEAXdoeKAgjazd3FsLnmHgAribW50v5rd09rw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

更多日期时间的判断，可以参考官方的手册：

https://blacksmithgu.github.io/obsidian-dataview/reference/literals/#Durations

## 按备注搜索

虽然笔记相关的属性很多，但是我们常用的至于文件名称file.name、文件日期file.ctime、file.mtime。

更多情况下，我们会在文件里添加备注，标记更多的信息。

### ◆metadata

obsidian可以通过【文档信息】功能，给文档添加备注的信息，给后续查询提供信息。

在笔记上点击【...】，选择【添加文档属性】，可以在文档顶部，添加任意的信息。

比如，给文档添加【是否完成】的属性，并设置类型为【复选框】

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwHzpYHUvfD0AIPv0MvVZBBEZxFPWYFPsicibWWf7Dl9WphlpkYzOSfZtA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这样，点击复选框，就可以把笔记标记成【完成】的状态。

文档属性支持6种不同的数据类型，大家可以根据需求选择。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwKwWKcicIT2yGFgntt2Hjl9YmG7e7VHcAicHraIyuXmUrIxtdaVMzK2Kw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### ◆是否完成

设置好文档属性后，可以按照属性来查询，比如下面的语句，用来查询勾选了【是否完成】复选框的笔记。

```
\`\`\`dataviewtablewhere 是否完成\`\`\`
```

查询结果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwPicEVbrTLCPYc1BhJWykjE5iajnmXj5RkaVkTkWfuAQYibmUpx5m5osIQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### ◆评分

在笔记中添加一个【评分】的属性，设置类型为数字。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwZRkibIeHT5T3gaHoc52yrbu0NhGnrEUvm4YjEM8dlu8N6iauDYzx2ib3w/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

使用下面的语句，可以把所有评分>3的笔记找出来，非常适合用来管理读书笔记、书评。

```
\`\`\`dataviewtablewhere 评分>3\`\`\`
```

### ◆备注

给文档添加【备注】的属性，输入文档相关的关键字。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwTexibMWTibWgn4GkuuWjlS6OKJ9hGgCSsguToLOUIs4cAWWeYic2NobqQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

后续，可以按照备注的内容，快速的检索关键字。

```
\`\`\`dataviewtablewhere contains(备注,"关键字")\`\`\`
```

## 按任务检索

dataview的where关键字，还可以用来检索任务。

### ◆未完成

下面的语句，可以把所有未完成的任务都罗列出来。

```
\`\`\`dataviewtaskwhere !completed\`\`\`
```

查询结果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwezkpf9NP4sFEQArNmXO4Biabu0ebvZcQANnnek499v7icOEtStUMo7sg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

【!completed】是固定的关键字，表示未完成的任务，相反的【completed】表示完成的任务。

### ◆本月未完成

结合这and关键字，可以进行多条件的筛选，比如下面的语句，用来查询出3月份完成的任务清单。

```
\`\`\`dataviewtaskwhere completed and dateformat(completion,"yyyyMM")="202403"\`\`\`
```

查询结果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwajdepAg7mhqeAxZRWFvY55SG2NkBb3G1mKyoiaJ279eAgUGKDwHRftQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Task还有很多的属性，比如：

- `completion`: 完成日期
- `due`: 计划完成日期
- `created`: 创建日期
- `start`: 任务计划开始日期
- `scheduled`: 周期等等

更多task相关的属性，可以参考官网的说明：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TKZI7weGVSAJ350sXAmLOwzyQeyB9TB6LPYUcnrcXliaKSRAHRgm8qwIv3GCPuvib78cnjcKKI1s0g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/

## 总结

为了更好的理解where关键字，我们可以和Excel中的筛选功能做类比。

- where，就是筛选功能。
- 文档属性，就是表格中的列标题。
- 逻辑判断，就是筛选时的条件，比如：大于、小于、等于、文本包含、开头是xx等等。

用好where关键字，搭建自己的固定筛选笔记，比如：

- 本周输出。筛选本周新建的笔记
- 常用笔记。筛选出指定关键字的笔记
- 本周待办。筛选出本周的任务列表等等

慢慢建立知识管理习惯，相信每个人都能成为学习高手！
