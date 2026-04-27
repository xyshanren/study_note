---
url: https://blog.csdn.net/sinat_41104353/article/details/82979852
source: blog.csdn.net
date_saved: 2026-04-23
tags: LLM, 后端, DevOps
content_type: article
word_count: 6000
status: 已处理
---

# Emacs插件管理与配置-CSDN博客

> source: [blog.csdn.net](https://blog.csdn.net/sinat_41104353/article/details/82979852)

## 核心要点

1. 有一点得提前说明：我使用的基本上是 陈斌的配置，所以以下也是在其原有结构上实现的
2. 然后我们可以使用 C-s 来搜索我们想要的插件。这里我以 org-download 为例，该插件是一个方便 org-mode 插入图片的插件，有了它我们就可以很方便的在编辑过程中将图片加入到我们的 org文件 中了
3. 如果没有找到我们想要的插件，可以在 init-elpa.el 中的 melpa-include-packages 变量中添加

## 内容预览

emacs 下载安装插件

 最新推荐文章于 2026-02-19 06:15:50 发布

 原创

 最新推荐文章于 2026-02-19 06:15:50 发布
 ·
 8.8k 阅读

 ·

 4

 ·

 14

 文章标签：

 #emacs
 #elpa
 #下载插件
 #emacs包管理器

 毫无疑问，使用 emacs 自带的 elpa 包管理器下载和管理插件是最便捷的方法

emacs 24之后的版本内置了 elpa 包管理功能，下面我来介绍如何使用 elpa 的包管理功能来方便的安装插件。
 有一点得提前说明：我使用的基本上是 陈斌的配置，所以以下也是在其原有结构上实现的。

 文章目录

 ELPA

 寻找并下载插件

 配置插件

 新建 `init-org-download.el`

 打开 `~\.emacs.d\lisp\init-elpa.el` （可省略）

 加载配置文件

 个人理解

 手动安装

 参考文献

ELPA

寻找并下载插件

我们打开 emacs 后使用 M-x list-packages 函数就可以列出所有可以安装的第三方包了。

然后我们可以使用 C-s 来搜索我们想要的插件。这里我以 org-download 为例，该插件是一个方便 org-mode 插入图片的插件，有了它...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
