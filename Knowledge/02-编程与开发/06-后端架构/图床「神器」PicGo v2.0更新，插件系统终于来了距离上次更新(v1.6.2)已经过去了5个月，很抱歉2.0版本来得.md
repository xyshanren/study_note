---
url: https://juejin.im/post/5c3b3bb8f265da612c5e1765
source: juejin.im
date_saved: 2026-04-23
tags: DevOps, 效率工具
content_type: article
word_count: 2254
status: 已处理
---

# 图床「神器」PicGo v2.0更新，插件系统终于来了距离上次更新(v1.6.2)已经过去了5个月，很抱歉2.0版本来得 - 掘金

> source: [juejin.im](https://juejin.im/post/5c3b3bb8f265da612c5e1765)

## 核心要点

1. 除了发布PicGo 2.0本体，一同发布的还有PicGo-Core（PicGo 2.0的底层，支持CLI和API调用），以及VSCode的PicGo插件vs-picgo等
2. 换句话说，如果你书写了合适的Uploader，那么可以上传到不同的图床。如果你书写了合适的Transformer，你可以通过URL先行下载文件再通过Uploader上传等等
3. 另外，如果你不想下载PicGo的electron版本，也可以通过npm安装picgo来实现命令行一键上传图片的快速体验

## 内容预览

图床「神器」PicGo v2.0更新，插件系统终于来了

 Molunerfinn

 2019-01-13

 5,253

 阅读4分钟

 前言

距离上次更新(v1.6.2)已经过去了5个月，很抱歉2.0版本来得这么晚。本来想着在18年12月（PicGo一周年的时候）发布2.0版本，但是无奈正值研究生开题期间，需要花费不少时间（不然毕不了业了T T），所以这个大版本姗姗来迟。不过从这个版本开始，正式支持插件系统，发挥你们的无限想象，PicGo也能成为一个极致的效率工具。

除了发布PicGo 2.0本体，一同发布的还有PicGo-Core（PicGo 2.0的底层，支持CLI和API调用），以及VSCode的PicGo插件vs-picgo等。

插件系统

PicGo的底层核心其实是PicGo-Core。这个核心主要就是一个流程系统。(它支持在Node.js环境下全局安装，可以通过命令行上传图片文件、也可以接入Node.js项目中调用api实现上传。)

PicGo-Core的上传流程如下：

Input一般是文件路径，经过Transformer读取信息，传入Uploader进行上传，最后通过 Output 输出结果。而插件可以接入三个生命周期（beforeTransform、beforeUpload、afterUpload）以及两种部件（Transformer和Uploa...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
