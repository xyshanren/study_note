---
url: https://cloud.tencent.com/developer/article/1437517
source: cloud.tencent.com
date_saved: 2026-04-23
tags: Python, 前端, 后端
content_type: article
word_count: 2793
status: 已处理
---

# vscode添加python文件头模板-腾讯云开发者社区-腾讯云

> source: [cloud.tencent.com](https://cloud.tencent.com/developer/article/1437517)

## 核心要点

1. https://cloud.tencent.com/developer/article/1437517 
pycharm可以自动生成python的文件头模板，但是vscode目前还不可以（不支持python，c的似乎有插件支持了）。琢磨了一下，可以通过用户代码片段来实现
2. 什么是用户代码片段
参考文章说的很详细：跟我一起在Visual Studio Code 添加自定义snippet（代码段）
2
3. 使用方法
在.PY文件上面输入header回车就会自动生成文件头。一般输入hea就会自动联想出来，

 效果图：

4

## 内容预览

锦小年

vscode添加python文件头模板

关注作者

腾讯云
开发者社区

文档建议反馈控制台登录/注册

首页学习
活动
专区
圈层
工具

MCP广场

文章/答案/技术大牛搜索
搜索关闭

发布

锦小年

社区首页 >专栏 >vscode添加python文件头模板

vscode添加python文件头模板

锦小年

关注

发布于 2019-05-28 20:00:20
发布于 2019-05-28 20:00:20
10.1K1

举报

文章被收录于专栏：锦小年的博客锦小年的博客

版权声明：本文为博主原创文章，未经博主允许不得转载。python版本为python3，实例都是经过实际验证。 https://cloud.tencent.com/developer/article/1437517 
pycharm可以自动生成python的文件头模板，但是vscode目前还不可以（不支持python，c的似乎有插件支持了）。琢磨了一下，可以通过用户代码片段来实现。
1. 什么是用户代码片段
参考文章说的很详细：跟我一起在Visual Studio Code 添加自定义snippet（代码段）
2. python头文件配置

 之后选择python后会生成python.json,将原来内容替换为一下内容：
代码语言：javascript

复制

{
 ...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
