---
title: Obsidian一键创建日报，一定要学会【模版】插件
source: https://mp.weixin.qq.com/s?__biz=MzA3MzA5MTk2MA==&mid=2649688758&idx=1&sn=6816292f65f706af53f6c6bad71e5faf&chksm=870ff4a1b0787db7739bbc59199597837d5289bbbb4cc87f297f7710811c3f28dd1d054232c4&cur_album_id=3189343285316665351&scene=189#wechat_redirect
author:
  - "[[拉登Dony]]"
published: 微信公众号
created: 2025-01-13
description: 今日目标：学习模版插件，一键创建日报Obsidian系列文章目录：第1天，从零开始学习Obsidian第2天
tags:
  - obsidian
---
返回目录页[[从零开始学习Obsidian]]

> **今日目标：**
> 
> 学习模版插件，一键创建日报

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



工作中有一件非常讨厌的事情就是**【写日报】**。

我们的日报里面要写5项内容，每天我都要把这5项内容的标题，反复的敲一遍，很浪费时间。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibIw9A0v2uShZOJMr6KHQB6B6ElLH4x4zD0Dqhotr2HdqTG6f9uAGVFA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我就在想有没有一种可能，打开OB就是这个日报的模板，直接往里敲就可以了。

当然可以！

**Obsidian的【日记】和【模板】功能，可以基于指定的模板，来快速创建笔记**，我们先来看模板的功能。

## 模版

## 开启模版插件

在Obsidian的【核心插件】里面开启【模板】功能。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibqynWEvHk342JucIJmxwdtdHhnALl9Q1zibNTQmicexz126VkbRUTM3pQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此时，在Obsidian的左边，可以看到一个【模版】的按钮，现在点击，会出现错误的提示。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibaVpuHAea8BezZwoibOEyaQ9iaM49sd6ZPIcenyBWq8qCWibfXtWcc2QKQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibfUhh6RFmf4psF0vM5AyeiaeObN5pMbEicXoXY8MyprUfzZYUBdhp4GKw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

因为模版有几个基本的选项需要设置一下。

## 设置【模版】选项

在Obsidian选项中，点击【模版】，设置基本选项。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibQN7q77ymHcse7PCxSRKc5vomGLpwM9uJsCbanhCUlDsGY6tiatf2FaQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### ◆文件夹

选择一个存放笔记模版的文件，后续【模版】功能，会在这个文件夹里，读取模版。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ib5ly0oKkyshDXEbAibqNQe3oEhwMolwL7tiakI3bnvxMAJhNp645AUDwQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这里我新建了一个template文件夹，用来保存笔记模版。

### ◆日期

设置日期的格式，可以在模版中使用{{date}}，来动态的获取当前的日期。

> 默认的日期是YYYY-MM-DD的格式，比如2023-12-13。

我们可以通过设置日期代码，来显示不同的格式的日期。比如：

设置成**MM.DD ddd**，那么{{date}}显示的会是**12.13 周三**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibcdw2OCgXBl3hms6xx8pbYDPgybkzaYzGBlepuBB4NyKukl9Zjur3xQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### ◆时间

和日期格式类似，在模版中使用{{time}}，可以动态获取当前时间。

可以时间代码来显示不同格式的时间，比如：

设置成**A h:mm:ss**，那么{{time}}会显示成**上午 11:28:20**

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibHdibs20fnzjqibVM43ur045YTrIEpKnOuVyXrYxhzsaYSw9tgiad0Biasg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

更多的代码设置，可以参考下面的网址：

https://momentjs.com/docs/#/displaying/format/

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibOtpJA7U71uCfasyg1wNPOeR6FUuxQ6mnYglLOGq3aNvUR19JNpkNNA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 模版举例1，日报

设置好了模版的选项之后，接下来就可以使用模版了。

在template中新建了一个【日报】的模版，模版中填写了日报的5个标题。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibeUicF7MS9DicfN3A93Xz4yjp011ia6R3Br2XCReSgUtw53n4ZFdczicQPQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

那么再写日报的时候，就可以用模版功能，一键创建了。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibdic1GH8FFibGgoJJvCibTOHEd3icOcwEtib4Unsbv9actrmhJGuKVS0Ag2w/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 模版举例2，高亮块

再比如说写笔记的时候，我经常需要用高亮块，非常的好看。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibMpvu09z2a6UMZge4ZpZRQmg8pxiaJzF8cFMauCMPtV5wUgdZSxf9IJQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

但是高亮块的创建太麻烦了，要写复杂的代码。

```
> [!note] 标题> 请输入笔记
```

那么，在template中新建一个笔记，输入高亮块代码，保存成模版。

然后只需要插入模板就可以一键快速创建高亮块。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibWH0b22cOFoedQssY2CNzyot5YAz0rL7VEGPwupzkDV5RD4UAL1dcXA/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 日记

【模板】功能可以让我们将文字模板快速插入到笔记里面。

但是日报要每天新建一个文档，那么每天的步骤还是有点繁琐：

1. 新建笔记
2. 设置笔记名称
3. 插入日报模版

这个时候就可以使用【日记】功能把上面的步骤，整合成一步。一键插入日报。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ib6Ebp4GZRGVTORArg6icRzqDDRiaEbWW6tkWjyq6ALkYDaXt8l0tVmJOQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 使用【日记】功能

首先在核心插件里面开启【日记】功能。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibCMnSC6Ll7bJ1xgcWQIrbq4qiaFCnV0PBRHfpmYywoHuasrP4s89EdjQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

设置一下日记的参数，包括

1. 文件的名称。日记笔记的名称
2. 日记存放位置。选择一个文件夹，存放新建的日记
3. 日记模板。要引用的笔记模版

下面是我的设置，日记名称是“日记”+当前的日期。默认存放在Daily文件夹中，引用“日报”模版。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibQgQujStb9JaI0jDzcRlnjI27RXVdsDm9IscfiaXtuxven46uJYvAhhw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

## 新建日期

设置好【日记】选项之后，回到Obsidian里面可以看到一个【日记】的按钮，点击按钮就可以一键创建日记，编辑日报。太高效了！

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ib6Ebp4GZRGVTORArg6icRzqDDRiaEbWW6tkWjyq6ALkYDaXt8l0tVmJOQ/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这样我们就可以快速写日记了

## 总结

学会了模版、日记功能，可以减少很多重复性的操作，提升笔记的效率。

## 更多技巧

效率提升这个事情，没有天花板。仁者见仁智者见智。

比如，我想把当前文件夹的笔记名称，都插入的当前笔记里，做成一个目录。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibVhU9qcz0nLpEG8RErtF5FC20ak0YFSyymEnWqBMDiaUVfpp5jGxo0gg/640?wx_fmt=gif&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这个需求用【模版】功能就实现不了了。只能借助更强大第三方插件【Templater】来实现。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/VpIHXp1jib5TOkwlqB8PnaqOnm6LG530ibJZzTEPOTryRehIk2fPMP67o4B6p2NPo4Kbiak1qzibBIkOM7hkfMfVFQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
