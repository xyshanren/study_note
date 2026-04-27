---
url: https://www.jianshu.com/p/36d92bea9f18
source: www.jianshu.com
date_saved: 2026-04-23
tags: Python, 后端, DevOps
content_type: article
word_count: 3152
status: 已处理
---

# Anaconda管理包及环境 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/36d92bea9f18)

## 核心要点

1. Anaconda管理包及环境
管理包

安装了 Anaconda 之后，管理包是相当简单的
2. 要安装包，可以直接在终端中键入 conda install package_name
3. 例如，要安装 numpy，请键入 conda install numpy

## 内容预览

Anaconda管理包及环境
管理包

安装了 Anaconda 之后，管理包是相当简单的。

要安装包，可以直接在终端中键入 conda install package_name。

例如，要安装 numpy，请键入 conda install numpy。

还可以同时安装多个包。

类似 conda install numpy scipy pandas的命令会同时安装所有这些包。

还可以通过添加版本号（例如 conda install numpy=1.10）来指定所需的包版本。

Conda 还会自动为你安装依赖项。例如，scipy 依赖于 numpy，因为它使用并需要 numpy。如果你只安装 scipy (conda install scipy)，则 conda 还会安装 numpy（如果尚未安装的话）。

要卸载包，使用 conda remove package_name。

要更新包，使用conda update package_name。

如果想更新环境中的所有包（这样做常常很有用），使用 conda update --all。

要列出已安装的包，使用conda list。

如果不知道要找的包的确切名称，可以使用 conda search search_term进行搜索。

例如，我知道我想安装 Beautiful Soup，但我不清楚确切的包名称。因此，可...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
