---
url: https://www.jianshu.com/p/0286b52df32a
source: www.jianshu.com
date_saved: 2026-04-23
tags: Python, 前端, 后端
content_type: article
word_count: 5635
status: 已处理
---

# 08 Django中render、render_to_response、HttpResponse、JsonResponse、Response的使用 - 简书

> source: [www.jianshu.com](https://www.jianshu.com/p/0286b52df32a)

## 核心要点

1. return render(request, 'blog_add.html',locals())

4 HttpResponse、JsonResponse、Response的使用

参考资料1：http://www.cnblogs.com/LYliangying/artic

## 内容预览

08 Django中render、render_to_response、HttpResponse、JsonResponse、Response的使用
Django作为一个后台框架，如何将数据正确的传递给前端呢？这得根据前端不同的数据请求方式，正确的使用render、render_to_response、HttpResponse、JsonResponse以及Response

1.1 场景一：传递数据给html，并直接渲染到网页上，使用render

from django.shortcuts import render

def main_page(request):
 data = [1,2,3,4]
 return render(request, 'index.html', {'data': data})

#html使用 {{ }} 来获取数据
<div>{{ data }}</div>

1.2 场景二：传递数据给js，使用render,但数据要json序列化

# -*- coding: utf-8 -*-

import json
from django.shortcuts import render

def main_page(request):
 list = ['view', 'Json', 'JS']
 return render(req...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
