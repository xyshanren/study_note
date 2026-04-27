---
url: https://juejin.im/post/5b9b5a6f6fb9a05d22728e36
source: juejin.im
date_saved: 2026-04-23
tags: 前端, 效率工具
content_type: article
word_count: 1140
status: 已处理
---

# 三分钟教你同步 Visual Studio Code 设置Visual Studio Code(以下简称vsCode)现 - 掘金

> source: [juejin.im](https://juejin.im/post/5b9b5a6f6fb9a05d22728e36)

## 核心要点

1. Visual Studio Code(以下简称vsCode)现在已经渐渐成为前端开发的主力工具，谁让它这么轻便，功能又这么丰富呢。
2. 我曾经换电脑的时候，把vsCode上自己心爱的插件一个个记下，然后去新电脑上重装，太蠢了。
3. 上传： Shift + Alt + U (Sync: Update / Upload Settings)
4. 如果快捷键有冲突，可Ctrl + K + S快捷键设置配置其它快捷键 或 Ctrl + P / F1 在命令窗口输入 >sync 即会出现相应命令供选择
5. (Sync: Update / Uplaod Settings) Shift + Alt + U 在弹窗里输入你的token， 回车后会生成syncSummary.
## 内容预览

三分钟教你同步 Visual Studio Code 设置

橘子味汽水_

2018-09-14

45,229

阅读2分钟

简介

Visual Studio Code(以下简称vsCode)现在已经渐渐成为前端开发的主力工具，谁让它这么轻便，功能又这么丰富呢。用vscode Coding的小伙伴们也一定会装很多插件吧。但是当你准备更换电脑的时候，是不是为迁移插件和设置而烦恼？我曾经换电脑的时候，把vsCode上自己心爱的插件一个个记下，然后去新电脑上重装，太蠢了。今天我就把vsCode同步设置和插件的方法告诉大家。

准备工作

下载Settings Sync插件

GitHub账号

1.安装Settings Sync

Setting Sync 快捷键：

上传： Shift + Alt + U (Sync: Update / Upload Settings)

下载： Shift + Alt + D (Sync: Download Settings)

如果快捷键有冲突，可Ctrl + K + S快捷键设置配置其它快捷键 或 Ctrl + P / F1 在命令窗口输入 >sync 即会出现相应命令供选择

2.打开GitHub

这样你就得到一个token，最好找个地方记下来。因为它就是同步设置的关键。

3.将token配置到vsCode

(Sync: Update / Uplaod Settings) Shift + Alt + U 在弹窗里输入你的token， 回车后会生成syncSummary.txt文件

syncSummary.txt文件会存储VSCode的设置及所安装的插件列表

如果你使用的是新版本的vsCode

打开设置，在搜索框中输入sync，就可以看到自己的token了

4.设置同步下载设置

(Sync: Download Settings) Shift + Alt + D 在弹窗里输入你的gist值，稍后片刻便可同步成功

5.如果要重置同步设置，变更其它token

Ctrl+P / F1 弹出输入>sync,即可重新配置你的其它token来同步

6.Tips

上传同步设置的时候，vsCode底下可以看到操作信息

Setting Sync 可同步包含的所有扩展和完整的用户文件夹

设置文件

快捷键设置文件

Launch File

Snippets Folder

VSCode 扩展设置

工作空间

博客同步更新

参考文章

Settings Sync更新说明

橘子味汽水_

公众号【18岁的前端程序员】 @某不知名上市公司

19

文章

80k

阅读

152

粉丝
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
