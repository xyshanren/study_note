---
url: https://www.cnblogs.com/xiepeixing/p/4243288.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端
content_type: article
word_count: 6000
status: 已处理
---

# [转]SpringMVC Controller介绍及常用注解 - xiepeixing - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/xiepeixing/p/4243288.html)

## 核心要点

1.    单单使用@Controller 标记在一个类上还不能真正意义上的说它就是SpringMVC 的一个控制器类，因为这个时候Spring 还不认识它。那么要如何做Spring 才能认识它呢？这个时候就需要我们把这个控制器类交给Spring 来管理
2. 这个时候有两种方式可以把MyController 交给Spring 管理，好让它能够识别我们标记的@Controller 
3.    第一种方式是在SpringMVC 的配置文件中定义MyController 的bean 对象

## 内容预览

Freiheit

Simple is beautiful

首页

订阅

-->

管理

 [转]SpringMVC Controller介绍及常用注解

一、简介

         在SpringMVC 中，控制器Controller 负责处理由DispatcherServlet 分发的请求，它把用户请求的数据经过业务处理层处理之后封装成一个Model ，然后再把该Model 返回给对应的View 进行展示。在SpringMVC 中提供了一个非常简便的定义Controller 的方法，你无需继承特定的类或实现特定的接口，只需使用@Controller 标记一个类是Controller ，然后使用@RequestMapping 和@RequestParam 等一些注解用以定义URL 请求和Controller 方法之间的映射，这样的Controller 就能被外界访问到。此外Controller 不会直接依赖于HttpServletRequest 和HttpServletResponse 等HttpServl...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
