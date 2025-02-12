---
title: Obsidian效率翻倍的模版插件，Templater
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649688792&idx=1&sn=fb1e815845abe19205cf89b6e64824fe&chksm=870ff4cfb0787dd924c15b58acd6b168c2241c1f3326ea8a32ee0da00af58264da470d1c86f5&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 今日目标：学习Templater插件，创建高级模版
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学习Templater插件，创建高级模版

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

今天是学习OB的第10天，上一篇教程里，我们讲了模板和日记的功能，可以从模版里，快速创建笔记。

今天要讲的是一个【高级版】的模版插件**Templater**。

在学习Obsidian的【模板】功能时，我们知道了{{date}}变量，可以在笔记里动态的创建日期。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEJz0k2uc34a9VibpKYvEAKbwElFWRjBfeORDvY2kic2a0fUPx7doPz8KQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

而Templater中可以使用更多的变量，来实现一些更高级的功能。

**比如我想把当前文件夹中，所有的笔记文件名称，导入到当前笔记，变成文章的导航。**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEE08xWXWhgxqBCiaWBGJibfrfyAlIib1KDt2B9VxficTywNq2R2sajr8d5Nw/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

是不是非常的酷，原本要复制粘贴好多次的工作，**一个快捷键就轻松搞定了。**

快点赞、收藏这个教程，我们一起学起来。

## Templater插件

### ◆安装和下载

首先在插件市场里面搜索Templater，下载并安装插件。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEiaBlqExvtPACUMO9XK997xqoAmTJbyh6Q6Dl07dEFOHQAuhX8ib05HwA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如果在安装过程中遇到问题，可以参考这篇文章。
[[Obsidian插件自学指南1-编辑器篇]]
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEF0BSOH4iaBKOThYb3Bbfzibb3fTY7Tx94IAeZlQayufvhjn2La6AH4Ow/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### ◆设置Templater

安装好之后，在Obsidian的设置选项里面，设置一下Templater的模版文件路径。

这里我设置了我已经创建好的template文件夹。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEIohULazljfSnfaXfJFN9OAz2icBoSXF7btmt4johHSj4iarGKnxSBhHQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 基础用法

在上一章中，我们在template文件夹里，已经创建好了两个模版文件：高亮块、日报。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEMFge3LvPBCMN8t52iaNxkOGSyicgfVPzXPplGPo0JicIL3zwaK8liaUAEA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

接下来，我们将使用Templater插件，来把这两个模版，快速插入到笔记中。

### 插入模版内容

回到任意笔记当中，可以按下【Alt+E】打开Templater的模板入口，选择准备好的模版【日报】【高亮块】，即可把模版内容，插入到笔记中。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEErIdAHaiaCUg6r5Aibr3QBkxVKeKXf3uP7JDPAibbfgDV9dmMbczjdicpQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这么来看，Templater插件，可以覆盖模版的所有功能。

## 进阶用法

### 1- 添加模版快捷键

接下来，是Templater的一些**进阶的功能**。

比如，为了更快速的调用模版，可以给**每个模板添加对应的快捷键。**

比如前面**高亮块**的模版，就不要引用模版，再选择高亮块了。

只需要按一下【Alt+1】就可以快速把高亮块模版，插入到笔记中去。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEED8Wv59hsSPIuVA9nbBdlfCvwutqAGsziahIg0agJI17yWs3A3I1kVAQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### ◆设置方法

具体设置方法如下：

1. 在Templater设置选项里面，点击【Add new hotkey for template】
2. 选择要添加的模板，比如这里的“高亮块”
3. 点击旁边的加号，搜索笔记名称，设置快捷键即可。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEd1nkwZTTpEC8lGHBMDSlAl5Rs5icFfEP6NPdicnLvVyPg7L09sH5kibGA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

同样的方法，我给添加笔记目录设置了【Alt+2】的快捷键，添加文件目录，变的更加丝滑了。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEE3ib7KQPEHdS4fK0ojiaqL9kHcCeFEQ0Pu9B3bsNjtbGWZaovVFDlQDtA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 2- 自定义模版变量

如何实现自动读取笔记文件的清单呢？

在template文件夹中，打开【目录】模版，查看笔记内容，就能发现秘密。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEBic2Cf016r9P9ZlcR4sRQRV1bPhYBSOHx0zmSJvoKIIOwK053h7975Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEE8C0ib8UhiaMykjIAPBUUkYtnEGWwR7afbfHhuBtxnPjO5OZiclaiaVZunw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

没错，这个模版文件里写了很多的代码，这个就是Templater的**高级功能**了。

Templater的模版文件里，插入一对儿**<% %>**符号，就可以添加自定义代码了，代码的语法和Javascript编程类似。

```
<%你的Templater代码%>
```

前面目录效果的完整代码如下：

```
<% tp.file.folder() %> 中的笔记清单：<% app.vault.getAllLoadedFiles().filter(x => (x.path.indexOf(tp.file.folder(true))>=0) && (x instanceof tp.obsidian.TFile)).map(x => "[[" + x.name +"]]").join("\n")%>
```

> [!NOTE] 备注
> 该代码有问题，后续有需要再研究下

使用方法如下：

#### ◆1- 新建模版文件

在模版文件夹里新建一个笔记。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEzZygt5y8B1mOgB3R8znPRDWxbkiabAwaMD2aPCpQhLbLmV7TUJVqAKQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### ◆2- 编写模版代码

打开笔记模版，粘贴下面的代码（前面已经给出，直接复制粘贴）。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEPzYksjtYn2xMIujXkLwm0gdtagmibHEcxkAur9jbcFduJtPiavpMDDiag/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

代码的作用，是读取当前笔记文件夹中的笔记文件清单。

#### ◆3- 使用模版

和其他普通的模版用法一样，按下【Alt+E】，选择刚创建好的模版即可。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEE0z3hQKVPHR6tHt7Nxtfjicd0880hHIOUZVECRTlsfjbWCicpTFw8RPSA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 3- 自定义案例

是不是很神奇？

我反正是没忍住这个好奇心，追到Templater的官网，研究了一天的代码。

https://silentvoid13.github.io/Templater/introduction.html

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEnCh2ZMTTN9JhhgfECGqB65ePEB2yDWNDVgl8eYorG3iamj2Sprjou8A/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

然后又写了一段代码，**可以搜索指定某个关键字的笔记，并把搜索到的结果，插入到笔记中去**。

效果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEErICdLIvkFDvm0otHxlbbMOn61hTm9Qesa0R0DibicF4nAc0wOLvldbEQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

对应的代码如下，用法和上一个案例一样。

```
<%*let my_word = await tp.system.prompt("输入要搜索的关键字：","")-%><% app.vault.getAllLoadedFiles().filter(x => (x.name.indexOf(my_word)>=0) && (x instanceof tp.obsidian.TFile)).map(x => "[[" + x.name +"]]").join("\n") %>
```

### 4- 更多代码

如果你有一定的编程基础，可以去Templater官网研究一下，还可以实现更多有趣的功能。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEERmRXg8x8Sm4urYVoaGEFrYrAdlwkYYjYgFib9RpCtjQEJ10KMs4k16Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

比如：新建笔记，移动笔记，复制笔记，删除笔记，笔记内容的合并，等等等等。

## 总结

虽然我花了一天的时间研究Templater插件，但是，**非常不推荐新手使用。**

- 简单的，用Obsidian自带的【模版】插件就能实现。
- 复杂的，学习成本太高了，不适合小白用户。

真的很难想象，一个笔记软件竟然还需要写代码，**这让人怎么能安下心来去写笔记呢？**

- 会代码的人，全都跑去写代码
- 不会代码的人，忍不住想去学代码

没人能专心写笔记内容，Templater是一个极其容易让你跑偏的插件。

## 笔记软件重要？还是方法重要？

前两天我在Obsidian官方社区里面看到了有趣的一个帖子：

**到底是笔记软件重要？还是笔记方法重要？**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEEQp2VeThLfn4anEaJfxRyow5HIibVf7DexG6KnZibQUb4FsaJJoq3XnCg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

评论区有一个张图片，我表示无比的赞同。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5Q2c09G1Ls312ZP64RFsqEESwlX96ticU3AlurW2BT0wIVIOyjxN1COUKYJWtCoSQo9TFQNibOLJYyw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

真正的大佬，什么都不用，笔记的核心是内容，而不是其他。
