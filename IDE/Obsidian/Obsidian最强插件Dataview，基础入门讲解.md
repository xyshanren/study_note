---
title: Obsidian最强插件Dataview，基础入门讲解
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649689379&idx=1&sn=b832900ec117ab9eb3a735c30706b40b&chksm=870ff334b0787a22e2b108aa03f8477303a91710b4b1f3aa577fa9a33d10b7f931cc046d553a&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 学习Dataview用法
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学习Dataview用法

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

**《从零开始学习Obsidian》系列教程，今天是学习OB的第17天！

---
## 回顾

之前我们学习Templater插件时，我编写了一段代码，制作了一个Templater模版，来快速导出当前文件夹中的笔记清单。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOvDIE7wYdoZTNiciacVVXiaGIOteoSEXNsiceIG9TQ8pJHBE8bO81I7DWzA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

当时，我还特意学习了JS代码，折腾了一整天。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOjIMAxdQaZpQ6uhWPibPUcFfYhAKqA6h2Y80YRUq4HZgoZCUuticPmqGQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

今天，我用上了Dataview插件，感觉自己之前就像一个傻瓜一样，还自己写代码。

同样的需求在Dataview中只需要一两句话，就可以轻松的把文件名称导出成表格、清单、日历等形式。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOfYicDHammC3qtIP87JrcAiaOZYPxrtQt1JYPvjx8ZgNplMOuq2oibuUzw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

还可以根据条件来筛选指定的笔记，怎么说呢？**如果你想把笔记导出到表格，实现类似Excel的筛选、排序等操作，那一定不能错过Dataview。**

接下来的几篇文章，会陆续学习Dataview的各种使用方法。今天，我们先来了解Dataview的入门使用。

## Dataview插件

### 下载安装

首先，在第三方插件市场下载并安装Dataview插件。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOTd7yy6ggafqZ1LCHVD1Rhb4WtkSJb402wfczjHYxLj6GhsPK9tr0ibw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果无法访问插件市场，可以通过我分享的链接下来。

- 链接：https://pan.quark.cn/s/baff6407cc0a

## 使用Dataview

和以往的插件不同，Dataview安装完成后，界面上不会有任何变化，我们无法通过鼠标操作来实现笔记导出，而是在笔记中编写查询语句，如下所示：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOoprWmNj2KSqE8ed8icafqiadbL1JHjau3kt3TKukS2586pQy2VN3hQ8A/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Dataview的查询语句整体很像SQL语句（一种数据库查询语言），内容大致包括：

1. 查询视图。List、Table、Task、Calendar
2. 查询条件。WHERE：基于笔记内部的信息、元数据字段过滤笔记。
3. 排序方式。SORT：根据字段和方向对您的结果进行排序。
4. 分类汇总。GROUP BY：将多个结果捆绑成一个结果行每组。
5. 指定数量。LIMIT：指定查询结果要返回的记录数。
6. 拆分填充。FLATTEN：基于字段或计算将一个结果拆分为多个结果。

看上去很复杂，不用担心，拉小登老师最擅长的就是化繁为简，用简单的类比、案例，讲解复杂的知识。

今天，我们现在来学习查询视图。

## 查询视图

### List清单视图

如果想要把Obsidian仓库中所有的笔记名称，都导出到笔记中，可以使用下面的代码：

```
\`\`\`dataviewlist\`\`\`
```

Dataview插件就把所有的笔记导入到当前笔记，点击笔记名称，可以直接跳转到对对应笔记。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOkAHfeasYayej94GQ5LkkZjqMHSzEtibHojpKoAWV1VNicuRW6OOqLz0g/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### Table表格视图

List只有一列，显示的信息非常的少，如果想要获取更多笔记信息，比如笔记创建时间、文件夹，可以使用Table视图。

在笔记中输入下面的代码，可以得到笔记的名称清单。

```
\`\`\`dataviewtable\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOVQj9Gexywd0LXkia4nrfmOnTicqmehKufkJbbHPWsww4l1EwLOiafVFHQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

表格中目前只有一列笔记名称，和list效果类似。如果需要添加更多信息，如文件夹名称、创建日期等，只需在后面用逗号间隔添加相应字段。

```
\`\`\`dataviewtable file.folder, file.cday\`\`\`
```

这样，我们就能快速输出一个包含文件路径和创建日期的表格，并能通过链接快速跳转到笔记。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOWghaATibHBM8Bgyx9AIZXOfa8KUu3MKu2euFEY0SoWCpUPdyXib9CCiaA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

更多文件的属性字段，已经有大佬帮我们整理好了，根据需求在table中添加字段名称就可以了。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOORvuG29gWGlBnmLBWRvA4Ns2tlkwKb56RNML8eYRZhop9alkzW3wCA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

*图片来自小红书：小白薯爱学习*

### Tasks任务视图

Dataview的强大之处在于，除了能够列出文件外，还能获取笔记中创建的所有待办事项清单。

输入\`tasks代码，就可以一键获取所有待办事项。

```
\`\`\`dataviewtask\`\`\`
```

你可以直接在查询结果中勾选待办事项进行标记完成，或点击链接进入原文档编辑。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfO3mTTILH0a21ibX46SSoJ8QQUF8iaWRmYCMRLx5l0Kd61u4rzSnjVRmPA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们可以根据条件查询，如未完成的待办事项，或者未完成的待办事项等等，这将在后续文章中展开。

### Calendar日历视图

Calendar日历视图让我印象最深刻。在Excel表格中创建日历表通常很复杂，在Dataview中，只需将查询类型改为`calendar`，就可以按日期输出一个直观的表格。

```
\`\`\`dataviewcalendar file.cday\`\`\`
```

上面的代码表示Dataview按照笔记创建日期，输出一个日历表。表格中的点代表特定日期创建的笔记数量，鼠标悬停可以预览笔记，点击则直接跳转到笔记中。这样回顾过去一周或一个月的笔记输出情况非常方便。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5RzHxkslD9SIwSZxaG0IlfOugo7monGHZfNn5OdgKGPsRxHC2fjeVgiaBOnVA2HHngckibMaKATob2A/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这个效果，真是让人爱不释手啊。

> 更多Dataview的语法，可以参考官网帮助文档，先自己预习一下。
> 
> https://blacksmithgu.github.io/obsidian-dataview

## 总结

Dataview的查询语句叫做DQL（Data query language），语法上和SQL语句非常像。

我们可以添加查询条件，选择查询来源，设置排序顺序等。这些涉及到一些复杂的查询语法，我们将在后面的文章中详细展开。
