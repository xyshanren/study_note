---
url: https://juejin.im/post/6844904063688179720
source: juejin.im
date_saved: 2026-04-23
tags: 前端, DevOps, 算法
content_type: article
word_count: 5148
status: 已处理
---

# nginx 配置 https有可能你当前已经通过 apt-get yum 等命令安装了，但是可能不支持 https ht - 掘金

> source: [juejin.im](https://juejin.im/post/6844904063688179720)

## 核心要点

1. nginx 配置 https

 卖好车大前端团队

 2020-02-13

 13,156

 阅读4分钟

 安装 nginx

有可能你当前已经通过 apt-get yum 等命令安装了，但是可能不支持 https http2 ipv6 等功能
2. 查看当前版本配置

我们可以通过 nginx -V 命令来查看版本以及支持的配置
3. 这里推荐 Let’s Encrypt 机构，然后使用 acme.sh 从 letsencrypt 生成免费的证书，且可以生成泛域名证书

## 内容预览

nginx 配置 https

 卖好车大前端团队

 2020-02-13

 13,156

 阅读4分钟

 安装 nginx

有可能你当前已经通过 apt-get yum 等命令安装了，但是可能不支持 https http2 ipv6 等功能。

查看当前版本配置

我们可以通过 nginx -V 命令来查看版本以及支持的配置。

下面这以 ubuntu 为例，卸载安装 nginx

卸载

# 移除 nginx
$ apt-get --purge remove nginx

# 查询 nginx 依赖的包，会列出来
$ dpkg --get-selections|grep nginx

# 移除上面列出的包，例如 nginx-common
$ apt-get --purge remove nginx-common

# 也可以执行 autoremove ，会自动删除不需要的包
$ apt-get autoremove

# 查询 nginx 相关的文件，删掉就可以了
$ sudo find / -name nginx*

安装

安装依赖库

# gcc g++
apt-get install build-essential
apt-get install libtool

# pcre
sudo apt-get install libpcre3 libpcre3-de...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
