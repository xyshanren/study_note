---
title: 精准管理你的知识库：Dataview插件的高级查询技巧
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649689405&idx=1&sn=0647eb866e85876b9cfb8291cc544fc9&chksm=870ff32ab0787a3c3640d76d5f629cf0c3602aee9ae5720745a416008f4ae4479e32c61f89eb&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 今日目标：设置Dataview数据源
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 设置Dataview数据源

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

**《从零开始学习Obsidian》系列教程，今天是学习OB的第17天！

---

上篇文章，我们学习了Dataview的基础用法，使用Table、List、Task、Calendar的视图，导出所有笔记的清单。

如果Obsidian中的笔记非常多，导出来的清单会非常的长，查询的效率并不高。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6ftftfZOVuCFRCzgZIbIqYexJqO2mBMyyyPbfz4tPVd65vP7jaaKeeg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

更多情况下，我们需要的是某个条件下的笔记，比如某个文件夹、某个标签中的笔记清单，该如何导出呢？

1. 查询视图。List、Table、Task、Calendar
2. **数据来源。文件夹、标签、链接、多条件**
3. 查询条件。WHERE：基于笔记内部的信息、元数据字段过滤笔记。
4. 排序方式。SORT：根据字段和方向对您的结果进行排序。
5. 分类汇总。GROUP BY：将多个结果捆绑成一个结果行每组。
6. 指定数量。LIMIT：指定查询结果要返回的记录数。
7. 拆分填充。FLATTEN：基于字段或计算将一个结果拆分为多个结果。

## 如何设置Dataview查询的来源，缩小检索的区域，更精准的导出笔记清单

### 文件夹

Dataview检索笔记清单，默认是整个Obsidian中的所有笔记，我们可以设置导出指定文件夹的笔记，使用关键字from来实现。

比如，在学习看板的时候，我把相关的笔记都放在了名为“第14天，看板”的文件中了。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6Wj2sqpUOT6lh8OO1vXkhTFHgfRHsh25aCU0pnpTaTCgwHRDxNia9jEw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

现在想要导出这些笔记的清单，对应的Dataview查询语句是：

```
\`\`\`dataviewtable file.cdayfrom "第14天，看板"\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p68RJ9gO1eNOSdKX2mxmrzlr11x6N7zOZY3F3iaviaLFpHQpmLamguR40w/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

使用过程中注意下面两点：

1. 文件夹的名称要用英文双引号包裹起来。
2. 支持子文件夹路径，比如“文件夹\\子文件夹”

### 标签

Obsidian中的标签功能，可以按照标签来查询对应的笔记，但是查询的结果除了文件名称，还包含了标签相关的文字信息，不够简洁。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6OZF8ZS4j8oAT0ORziaCTsN3Zp4TpNNZlcUVjicnxFKd2wY1pkIWISVvg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Dataview支持导出某个标签下面的所有笔记，对应的代码如下：

```
\`\`\`dataviewtablefrom #完成 \`\`\`
```

在编写代码过程中，输入#会自动出现标签的列表，方便我们选择。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6eQGVyZqia3zdD9w2Fd6mxqBC0SrgjJia7GR7KLFzwoznrzyoeLr2aXicg/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 链接

Obsidian的双链功能可以快速的跳转引用的文件，非常的方便。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6slv3gmRN0e00edNTNr7yu4GvsgShficGJWSZ9H46efFjztoIcNaeCJg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这些链接可能散落在文章的各个段落中，如果把链接都整理到文章底部，会更加美观，除了手动一个个复制粘贴引用，使用Dataview导出双向链接，更为方便。

#### ◆导出反向链接

和插入超链接类似，在Dataview中使用\[\[笔记名称\]\]就可以导出指定笔记中的反向链接。

```
\`\`\`dataviewtablefrom [[第4天/第1个笔记|第1个笔记]]\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p63JGcic1NbEDrXbBKJamFgQs46HoCxYoEffOCAmTU1BeyWPrcic8o2iajA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### ◆导出链接

在笔记名称前面加上`outgoing()`，就可以导出笔记中的外链。

```
\`\`\`dataviewtablefrom outgoing([[第4天/第1个笔记|第1个笔记]])\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6oFiafCVN94thcuE6mvK8l4W7hBhBZcvzhicCgAHZDKUic81Cb33guHVLw/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 多条件

掌握了上面集中查询的来源之后，我们可以结合and和or实现多个条件的查询。

比如，我想要把文件夹【第4天】中，包含【标签】标签的笔记，就可以这样进行查询。

```
\`\`\`dataviewtablefrom #标签 and "第4天"\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5SgZ4hIMgXNgOadwavQw7p6syD5cbkNtkc4iaicq01gtdnq3l5cB5qJo9jvjUHsbibZu2ooZN3iboibY2w/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

代码中，使用AND函数设置了同时两个条件同时的查询条件，并导出对应的笔记名称。

## 总结

通过FROM关键字设置查询的数据源，只能按照文件夹、标签、链接来查询笔记，如果想要按照笔记多内容，导出包含某个关键字的笔记，该怎么办呢？

那就要用到更为专业的条件语句Where了，具体怎么做呢？下一篇文章给大家揭晓。
