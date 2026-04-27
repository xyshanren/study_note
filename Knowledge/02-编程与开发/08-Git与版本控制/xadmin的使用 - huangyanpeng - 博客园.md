---
url: https://www.cnblogs.com/pgxpython/p/10217888.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: Python
content_type: article
word_count: 6000
status: 已处理
---

# xadmin的使用 - huangyanpeng - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/pgxpython/p/10217888.html)

## 核心要点

1. huangyanpeng

博客园

首页

新随笔

联系

订阅

-->

管理

 xadmin的使用

01-下载源码

GitHub地址：https://github.com/sshwsfc/xadmin

# 安装xadmin

由于使用的是Django2.0的版本，所以需要安装xadmin项目django2分支的代码
2. list_display 列表展示的字段

preserve_filters 默认情况下，当你对目标进行创建、编辑或删除操作后，页面会依然保持原来的过滤状态。将preserve_filters设为False后，则会返回未过滤状态
3. prepopulated_fields 设置预填充字段。不接收DateTimeField、ForeignKey和ManyToManyField类型的字段

## 内容预览

huangyanpeng

博客园

首页

新随笔

联系

订阅

-->

管理

 xadmin的使用

01-下载源码

GitHub地址：https://github.com/sshwsfc/xadmin

# 安装xadmin

由于使用的是Django2.0的版本，所以需要安装xadmin项目django2分支的代码。 
在PyCharm里打开命令行工具，输入以下命令完成安装：
pip install git+git://github.com/sshwsfc/xadmin.git@django2

也可以使用https的地址安装，命令如下：
pip install git+https://github.com/sshwsfc/xadmin.git@django2

02-配置settings.py

# 引入下面三个app
INSTALLED_APPS = [
 ....
 'xadmin',
 'crispy_forms',
 'reversion', 
]

# 修改使用中文界面
LANGUAGE_CODE = 'zh-Hans'

# 修改时区
TIME_ZONE = 'Asia/Shanghai'

ALLOWED_HOSTS = ['*', ]

03-配置路由

# urls.py

# -*- coding: utf-8 -*-
# from djan...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
