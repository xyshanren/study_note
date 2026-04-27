---
url: https://juejin.cn/post/7261808673034125371
source: juejin.cn
date_saved: 2026-04-23
tags: 前端, DevOps, 生物信息学
content_type: article
word_count: 663
status: 已处理
---

# windows系统中查看文件夹大小的几种方式使用gitBash工具和powershell工具（windows自带工具）在 - 掘金

> source: [juejin.cn](https://juejin.cn/post/7261808673034125371)

## 核心要点

1. windows系统中查看文件夹大小的几种方式

 小鱼儿亮亮

 2023-07-31

 4,065

 阅读1分钟

linux系统中可以直接使用命令查看文件夹大小，但windows系统中则比较麻烦。接下来就说一下几种查看方法
2. 一、gitBash工具

在windows系统中可以使用管理员权限在gitBash工具使用命令查看。命令如下：

$ du -sh *

二、powershell工具（windows自带工具）

ls命令默认只能显示文件的大小，无法显示文件夹大小
3. 使用powershell命令查看文件夹及文件大小（不需要管理员权限）

## 内容预览

windows系统中查看文件夹大小的几种方式

 小鱼儿亮亮

 2023-07-31

 4,065

 阅读1分钟

linux系统中可以直接使用命令查看文件夹大小，但windows系统中则比较麻烦。接下来就说一下几种查看方法。

一、gitBash工具

在windows系统中可以使用管理员权限在gitBash工具使用命令查看。命令如下：

$ du -sh *

二、powershell工具（windows自带工具）

ls命令默认只能显示文件的大小，无法显示文件夹大小。

使用powershell命令查看文件夹及文件大小（不需要管理员权限）。

Get-ChildItem |
Format-Table -AutoSize Mode, LastWriteTime, Name,
 @{ Label="Length(M)"; alignment="Left";
 Expression={
 if($_.PSIsContainer -eq $True)
 {(New-Object -com Scripting.FileSystemObject).GetFolder( $_.FullName).Size/1024/1024}
 else
 {$_.Length/1024/1024}
 }
 };

经对比，powershell效率更高一些，gitBash计算的会慢一些。（有明显的卡...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
