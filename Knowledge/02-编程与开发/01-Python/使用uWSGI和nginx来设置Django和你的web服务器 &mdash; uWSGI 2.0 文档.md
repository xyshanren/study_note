---
url: https://uwsgi-docs-zh.readthedocs.io/zh_CN/latest/tutorials/Django_and_nginx.html
source: uwsgi-docs-zh.readthedocs.io
date_saved: 2026-04-23
tags: Python, DevOps, 设计
content_type: documentation
word_count: 6000
status: 已处理
---

# 使用uWSGI和nginx来设置Django和你的web服务器 &mdash; uWSGI 2.0 文档

> source: [uwsgi-docs-zh.readthedocs.io](https://uwsgi-docs-zh.readthedocs.io/zh_CN/latest/tutorials/Django_and_nginx.html)

## 核心要点

1. Django 是一个高层次的Python Web框架，鼓励快速开发和干净实用的设计
2. nginx (发音为 engine-x) 是一个免费开源并且高性能的HTTP服务器和反向代理，还是一个IMAP/POP3代理服务器
3. 本教程的一些注释¶

注意

这是一个 教程 。它并不打算提供一个参考指南，对于部署主题，不要想着有一个详尽的参考

## 内容预览

Docs &raquo;

 使用uWSGI和nginx来设置Django和你的web服务器

 Edit on GitHub

使用uWSGI和nginx来设置Django和你的web服务器¶

本教程针对那些想要设置一个生产web服务器的Django用户。它介绍了设置Django以使得其与uWSGI和nginx工作良好的必要步骤。它涵盖了所有三个组成部分，提供了一个web应用和服务器软件的完整栈。

Django 是一个高层次的Python Web框架，鼓励快速开发和干净实用的设计。

nginx (发音为 engine-x) 是一个免费开源并且高性能的HTTP服务器和反向代理，还是一个IMAP/POP3代理服务器。

本教程的一些注释¶

注意

这是一个 教程 。它并不打算提供一个参考指南，对于部署主题，不要想着有一个详尽的参考。

对于Django部署而言，nginx和uWSGI是不错的选择，但它们并非唯一的选择，也不是“官方”选择。对于它们两个，都有不错的替代品，因此鼓励你去详细研究一下。

我们这里部署Django的方式是种不错的方式，但它 不是 唯一 的方式；
对于某些目的，它甚至也许不是最好的方式。

然而，它是一种可靠而简单的方式，而这里所涉及的材料将会向你介绍无论你用什么软件来部署Django都会熟悉的概念和过程。通过为你提供一个可用的步骤，以及向你预演完成此...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
