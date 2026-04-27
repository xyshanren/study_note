---
url: https://jingyan.baidu.com/article/359911f552827957fe0306f8.html
source: jingyan.baidu.com
date_saved: 2026-04-23
tags: DevOps
content_type: article
word_count: 1481
status: 已处理
---

# TortoiseGit状态图标不能正常显示的解决办法-百度经验

> source: [jingyan.baidu.com](https://jingyan.baidu.com/article/359911f552827957fe0306f8.html)

## 核心要点

1. 工具/原料
TortoiseGit

方法/步骤
1
确认是不是64bit 系统上装了 32bit 的 TortoiseGit,如果是的话，这个只要再安装 64bit 的 TortoiseGit就可以 了，如果不是，请往下看
2. 2
在开始菜单的搜索处，输入"regedit"命令
3. 3
在弹出的注册表编辑器中找到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers这一项

## 内容预览

百度经验 > 游戏/数码 > 电脑 > 电脑软件

TortoiseGit状态图标不能正常显示的解决办法

原创
|
浏览：39315
|
更新：2015-05-15 21:01
|
标签：it 

1

2

3

4

5

6

分步阅读

如果你安装 TortoiseGit之后，发现文件夹或文件左上角就是不显示图标，那么以下步骤就是最好的解决办法。

工具/原料
TortoiseGit

方法/步骤
1
确认是不是64bit 系统上装了 32bit 的 TortoiseGit,如果是的话，这个只要再安装 64bit 的 TortoiseGit就可以 了，如果不是，请往下看。

2
在开始菜单的搜索处，输入"regedit"命令。

3
在弹出的注册表编辑器中找到HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers这一项。

4
找到后可以发现在该项下有很多个，而Windows Explorer Shell 支持的 Overlay Icon 最多 15 个，Windows 自身使用了 4 个，只剩 11 个可扩展使用。

5
编辑...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
