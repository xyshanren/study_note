---
title: Obsidian插件：掌握Dataview的排序、限制和分组功能
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649689527&idx=1&sn=97c991fc15eeb2ce3c9ca8755089a3ec&chksm=870ff3a0b0787ab688fa24a0860d225c339affa1f5305152f5129edb200cfc275a13668c4dd3&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 今日目标：学会dataview的排序、分组
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学会dataview的排序、分组

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
>
>20- [[效率翻倍：掌握Dataview查询条件，轻松管理您的知识库]]

**《从零开始学习Obsidian》系列教程，今天是学习OB的第19天！

---

第16~18天的文章中，我们学习了Dataview的基本用法。今天，我们将继续学习dataview的另外3个用法，以高效地管理你的笔记。

## 1- 排序

你平时找文件都怎么做？Ctrl+F查找文件名？

其实还有更快捷的方法，在文件的访问记录里查看最近完成的笔记，也是一个不错的方法。

使用下面的Dataview语句，可以把所有的笔记，按照最近修改时间排序查询出来。

```
\`\`\`dataviewtable file.mtimesort file.mtime desc\`\`\`
```

效果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QHCegfnd98AhGn0QaqLsBsTljbzAutLlZBuKQes5zjCyMGc8iapmOEPzSb6B5z2dPbv6vNkQvUHmg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

代码大致解释如下：

### file.mtime

表示笔记最近修改的时间。

如果你的时间不是这个样子的，可以在dataview的设置选项中，修改日期时间的格式。

```
yyyy-MM-dd hh:mm
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QHCegfnd98AhGn0QaqLsBsibHiaxaDoDuEJ2ibr53xEETjZ4q13qGx8icPkmawibeiclxBWia9qEd5TpOHQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

输入下面的日期时间格式，就可以得到和我一样的效果了。

### sort

设置查询结果的排序顺序，语法如下：

```
sort 字段 排序方式
```

- 字段。就是笔记的属性如file.ctime, file.mtime，或者自定义的文档属性
- 排序方式。只能二选一，可以是ASC升序，也可以是DESC降序。

## 2- 限制数量

现在查询出来的笔记数量可能仍然很多，这时可以使用`limit`函数来限制结果数量。

在查询语句中加入`limit 10`，就可以找出符合条件的前10条数据。

```
\`\`\`dataviewtable file.mtimesort file.mtime desclimit 10\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QHCegfnd98AhGn0QaqLsBsjoERWzC0zic1cLqUCJu0uVBbUZ73a9jGbgyWaYudMezfshMH8NM1hNg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

limit就是限制显示结果的，后面的数字代表限制的记录数量。很简单吧，一看就懂。

## 3- 分组

虽然已经找出了最近10个笔记，但是每个笔记是哪个文件夹里的呢？能不能按照笔记文件夹分组呢？

当然可以，在代码后面加上group by就可以实现，代码如下：

```
\`\`\`dataviewtable file.mtimesort file.mtime desclimit 10group by file.folder\`\`\`
```

效果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QHCegfnd98AhGn0QaqLsBs2qSicG1hYDPAbumg1Gs9UvROXgtw0VWXDGQzT3mAw07ibXdPzuaAUKibg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

需要注意的是，默认情况下，只显示分组字段的内容，也就是笔记的文件名称。

如果你想在分组结果中显示对应的文件名称，需要在文件名称字段中加入`rows`，这样就会在每个分组中显示该分组下的文件链接。

代码修改如下：

```
\`\`\`dataviewtable rows.file.link, rows.file.mtimesort file.mtime desclimit 10group by file.folder\`\`\`
```
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QHCegfnd98AhGn0QaqLsBsibOvicnVUkVXicadIia0CibmyKpLV6VU9D8VcZ2usdt0OJZKECGWRSZL33g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

group by的用法，有点像Excel中的透视表，把字段拖到行区域，就实现了分组的统计。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QHCegfnd98AhGn0QaqLsBsycEmjHEkhbia57uwjIMsM5iaaKDnrEIzASc3sQzbgT1C317Q973eutQQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 总结

到这里，dataview的学习基本告一段落了，当然还有进阶的学习，比如可以使用JavaScript编写高级查询语句，实现更复杂的查询。

```
\`\`\`dataviewjslet pages = dv.pages("#books and -#books/finished").where(b => b.rating >= 7);for (let group of pages.groupBy(b => b.genre)) {   dv.header(3, group.key);   dv.list(group.rows.file.name);}\`\`\`
```

对于普通人这个学习成本就太高了，一般人的需求也不会那么的深入。

**更重要的是理解dataview插件的底层逻辑、根本需求。**

我感觉Dataview就是这么一回事。

1. 就像是在笔记软件中插入了一个表格
2. 自动提取了笔记的名称
3. 通过通过DQL（Dataview查询语言）语句，实现表格的筛选和排序功能。

**本质上dataview就是基于表格的思路，管理笔记、筛选笔记、更快速的访问笔记。**

理解了这个本质，这样才能触类旁通，即使换了笔记软件，也能更合理、更高效地管理我的笔记。
