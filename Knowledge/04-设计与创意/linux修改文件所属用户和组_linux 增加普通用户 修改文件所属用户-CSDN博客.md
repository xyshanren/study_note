---
url: http://blog.csdn.net/yun90/article/details/46911663
source: blog.csdn.net
date_saved: 2026-04-23
tags: 后端, DevOps, 设计
content_type: article
word_count: 4641
status: 已处理
---

# linux修改文件所属用户和组_linux 增加普通用户 修改文件所属用户-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/yun90/article/details/46911663)

## 核心要点

1. Linux
 专栏收录该内容

 23 篇文章

 订阅专栏

 本文详细介绍了如何使用Linux系统中的chown和chgrp命令来修改文件或目录的所有者和所属组，包括具体命令用法及实例演示
2. 福利倒计时

 :

 :

 立减 ¥

 普通VIP年卡可用

 立即使用

 _7爷

 关注
 关注

 0

 点赞

 踩

 1

 收藏

 觉得还不错
3. 精选资源

 Linux修改用户所属组的方法

 01-09

 - **chown** 和 **chgrp**：用于更改文件或目录的所有者和所属组

## 内容预览

linux修改文件所属用户和组

 最新推荐文章于 2025-03-22 19:44:35 发布

 原创

 最新推荐文章于 2025-03-22 19:44:35 发布
 ·
 6.8k 阅读

 ·

 0

 ·

 1

 ·

 CC 4.0 BY-SA版权

 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

 Linux
 专栏收录该内容

 23 篇文章

 订阅专栏

 本文详细介绍了如何使用Linux系统中的chown和chgrp命令来修改文件或目录的所有者和所属组，包括具体命令用法及实例演示。

 使用chown命令可以修改文件或目录所属的用户

       命令chown 用户 目录或文件名

       例如chown qq /home/qq  (把home目录下的qq目录的拥有者改为qq用户) 

使用chgrp命令可以修改文件或目录所属的组

       命令chgrp 组 目录或文件名

       例如chgrp qq /home/qq  (把home目录下的qq目录的所属组改为qq组)

添加用户所属的组 usermod -a -...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
