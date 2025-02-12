---
title: "Obsidian插件自学指南1-编辑器篇"
source: "https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649688058&idx=1&sn=403eb17b45c9e777cf815c99639e9ff8&chksm=870ff9edb07870fb1dfc97c210535a1745b08367bc79c480ec40e567864cade8a8f144ff3ac3&cur_album_id=3189343285316665351&scene=189#wechat_redirect"
author:
  - "[[拉登Dony]]"
published:
created: 2025-01-13
description: "今日目标：学习Obsidian的编辑器插件"
tags:
  - "obsidian"
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学习Obsidian的编辑器插件

前面的文章我们了解了Obsidian的基础的用法，如何创建笔记，如何做文件和内容的分类的管理等等。

> 1- [[初识Obsidian]]
> 
> 2- [[从认识界面开始]]
> 
> 3- [[Obsidian笔记分类技巧]]
> 
> 4- [[Obsidian标签管理]]

**今天我们开始正式学习Obsidian的插件。**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxNkpuWIeApLk841Gticr5o81O0P0ySwcsKXqMOmYmwLvRFMjVcMehumw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

但是开始之前，我们先来定好插件的学习的基调，避免迷失在obsidian的插件海洋中，只玩插件不写笔记，就本末倒置了。

所以在学习插件之前，先问问自己：

> \- 笔记的数量超过50个了吗？
> 
> \- 笔记的总字数，有超过10000字吗？

如果还没达到，老老实实的写笔记，否则一旦接触了插件，你会离写笔记越来越远。

## 如何使用插件

首先了解obsidian的插件安装方法。

### 1- 社区插件市场

首先，按照下图所示，进入到obsidian的第三方插件市场。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxicJBqzbLSX1DsoqYUm4j3BibZfjmagsKteicFjSIxias45JEJia6EE51fJw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

在左上角搜索插件后，点击【安装】即可。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibx02iaAp9h0450Kcuzq2D2Zj60uRvsNxufsgKFsjZnsAP5ia8AHUrJndCQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

安装后，还要点击【启用】，才可以正常的使用插件。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxRS0DxicBtntialyXhibLNqicma79yl3JeBphas7fuWH0ianS3y9f0qhcPfA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> 如果obsidian社区插件市场加载不出来，可以参考下面的链接来解决。
> 
> https://www.zhihu.com/question/496088793

### 2- 手动安装

有些插件在obsidian的插件市场里找不到，可以下载插件后，手动的安装。

> 比如后面会介绍的【增强编辑】插件。

下载后的插件文件夹（打开后是代码文件那个文件夹），拷贝到obsidian的插件目录里即可。

> obsidian的插件目录，在仓库文件夹下面的.obsidian\\plugins目录中。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxgHSKJfbBcrF3mF8QXjEaGPnsAnichvQu9KGZEicDboepbRIabAHp2uJA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

安装后，进入到obsidian的选项中，启用该插件即可。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxUiaa4FI0BrAf76z79EVEWcVBVgc9AC7y4XxOtkGQvhml0D6hbuwd4uA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

再多说一嘴，不要做插件的小白鼠，插件最终也是解决问题的，学习插件最好是以解决问题为目标，这些问题大致分为下面几类：

> 1- 编辑器类
> 
> 2- 笔记格式类
> 
> 3- 导入同步类
> 
> 4- 计划管理类
> 
> 5- 快捷操作类
> 
> 6- 其他

后面我们慢慢展开。

## 编辑器类插件

我们从编辑器类的插件说起。

编辑器的基本需求，就是打字、设置字体样式，段落的排版。

这些功能Obsidian都已经满足了，但是obsidian默认支持Markdown，只能通过MD语法，使用固定的样式，习惯了使用Word的同学，会感觉很别扭，因为：

> 1- 字体大小、字体颜色改不了。
> 
> 2- 行高、段落间距改不了了。
> 
> 3- 文字对齐方式改不了。
> 
> 4- 上标下标也没有。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxaZ0D8UkNqUElbU2UtvU4Kklt6icTB0BnhlWj7iaaToztvZJMfzg2IkYw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

下面介绍的几个插件，能够帮你把obsidian当成word来用。

### 1- Editing toolbar

插件的功能很简单，在笔记的顶部增加一个字体样式的功能按钮，可以方便的调整。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxSYekiagS3icMGDibTc5F7cFbGhLpOMfqxj1LQLZSMwZWypg9Vhw6v5TJg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 调整工具栏位置

如果你喜欢选中文本设置字体样式，可以按照下面的设置，把toolbar设置成喜欢的格式。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibx43McnlWfWXT9eKdRzroWdpv2coShvjXZiaQicT03L5ELU7wGukWaekSA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

修改后效果如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxttrRzQawaiao1tQT1FaqSQpD8JKFgDjyFFWDl8icJSkA9Edau6PnMJiaQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 添加自定义按钮

另外，editing toolbar还允许我们自己往功能栏中添加按钮。

点击【添加】按钮后，在命令框中选择指定的命令（比如分屏），然后设置一个图标，即可完成按钮添加。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxgG8oQtZCKB4Nh4jFWcPuibyIqMibxeL0xrGD7Pg10QJbKYnT4JL3AKyQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

接下来，在功能栏中点击按钮，就可以执行该命令，实现笔记快速分屏。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxiaS7mqyjkDSaYZqicbhkfOt0GBrBzicQN7o1rq4HX2WhjlTcdVOzPdCKA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 2- cMenu

还有一个和Editing Toolbar类似的插件，叫做cMenu，也有不少博主推荐。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxT4CUy08HxkbK7KZ4UeV0qVXXTt4IXFQqynPCv3XFgsGXwBhbsYhqHQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我研究了一上午，两个插件的功能基本一致，保留任意一个即可。

后来无意间看到Editing Toolbar的介绍，modified from cmenu。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibx8ibCQPxibsBdicfyRmY4LGG9icGGpQhpzQ89lRGYxPTesaORNg4FSNIMag/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

好吧，大家使用Editing toolbar即可。

### 3- 增强编辑

被称作是地表最强obsidian插件，是我们国内开发者开发的一个编译器增强的插件。

> 这个插件，无法在obsidian插件市场安装，需要访问链接下载，然后手动安装。
> 
> 方法1：作者的GitHub
> 
> https://github.com/obsidian-canzi/Enhanced-editing
> 
> 方法2：百度网盘
> 
> 链接: https://pan.baidu.com/s/1AUgEkp5-nXXCzoUOaMxwGQ 提取码: neiq

前两个插件通过添加功能按钮，实现字体的样式设置。

【增强编辑】主要是通过快捷键，更全面更系统的提升编写体验，我们列举几个比较有代表性的功能。

#### 「Alt+Z」转换内部链接

想要新建一个引用的文章，总是要输入四个符号\[\[\]\]，再输入标题。

使用了插件后，选择文字，按下「Alt+Z」一键转换成文章引用，点击自动新建文章。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxJrrxxnOuWQvOYDHUjKNuR42H1InstCByuhA2VnX3Ctdl83KlFibTsmw/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 「Alt+;」括号补全

输入特殊括号时，总要花时间去补全另一半。

使用增强插件的「Alt+;」可以一键补全括号，同时特殊符号可以使用一些组合字符，快速转换。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxS5Q49XyKHXyuaLAUXWROWmOW5CEsnDhaiaXsKCRePggbicTQwlY6ftnw/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 格式刷功能

在Office软件中，使用格式刷功能，可以把样式快速复制到其他的文字、对象、或者单元格上。

编辑增强中的格式刷功能，也Office软件中格式刷功能一样，开启了某个格式刷后，选择文本，就可以把文本自动设置成对应的样式，极大的提升了排版的效率。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibx3ER2HHrGkff1WBvW0s8Nq6LRctxgPEnZUBVuDrlsT2uJXfLV68AGsQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

编辑增强的功能还有很多，和【Editing toolbar】几乎没有重叠，对编辑功能有极值追求的同学，可以挨个尝试一下。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxITWnia1z7X6cMngJPCZftBmJmVGtSvBKZHwTqUTW4toicXiaDz8dhta5g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 4- Typerwrite scroll

这个插件有点像是【奢侈品】级别的编辑需求了，把打字的过程，模拟成复古打字机的那种感觉。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxudC3a3oRDynVsTPnAubDmIvRnmsWz8nt7tMp5lg45uL9oTBcKjrLfQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

通常情况下，按下回车光标会移动到下一行。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxq5crVqffsWf6IRvayscoy4SOCPG7NW5akt8PhpFCrA9d7l65JBx8xA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Typerwrite scroll的作用，则是保证光标位置不变，按下enter后，文字向上移动，模拟出老式打字机的效果。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxicwa2YZUZ3bksekvv9YHGdtXrpsDgIvRYDUuGJ91HUSbePQ5ibZK52Ug/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

估计用这个插件的人，没几个真正坚持写笔记的吧。

## 总结

针对编辑器的插件，常用的差不多就这么几个。主要优化了2个方面：

> 1- 类似Word字体样式功能按钮【Editing toolbar】和【cMenu】
> 
> 2- 自动化处理文字样式、段落排版、字符输入的【编辑增强】

关于插件再啰嗦两句。

Obsidian插件，就像是一辆跑车，先别着急买跑车，研究怎么飘逸、百米加速啥的，先把驾照考下来。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5QVkxZTh22uRiaiaArvz7L3ibxdugh7J4XNklHoyCD0yuom40ERLkfhE58Z0mBX5a2lNzffyibniatWoow/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

从上班的实用性来看，我会选择电动自行车。