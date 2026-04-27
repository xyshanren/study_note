---
url: http://www.cnblogs.com/qifengshi/p/6036870.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端, 效率工具
content_type: article
word_count: 3365
status: 已处理
---

# idea打包jar的多种方式 - byhieg - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/qifengshi/p/6036870.html)

## 核心要点

1. 进入Create JAR from Modules页面，按照如下图配置
2. 第二步选择如图的选项，目的是对第三方Jar包打包时做额外的配置，如果不做额外的配置可不选这个选项（但不保证打包成功）

第三步需要在src/main目录下，新建一个resources目录，将MANIFEST.MF文件保存在这里面，因为如果用默认缺省值的话，在IDEA12版本下会有bug
3. 点击OK之后，出现如下图界面，右键点击<output root>，点击Create Directory,创建一个libs，将所有的第三方JAR放进libs目录下

## 内容预览

Android孤独之旅

博客园

首页

新随笔

联系

-->

管理

 idea打包jar的多种方式

这里总结出用IDEA打包jar包的多种方式，以后的项目打包Jar包可以参考如下形式：

用IDEA自带的打包形式

用Maven插件maven-shade-plugin打包

用Maven插件maven-assembly-plugin打包

用IDEA自带的打包方式：

打开IDEA的file -> Project Structure，进入项目配置页面。如下图：

点击Artifacts，进入Artifacts配置页面，点击 + ，选择如下图的选项。

进入Create JAR from Modules页面，按照如下图配置。

第一步选择Main函数执行的类。

第二步选择如图的选项，目的是对第三方Jar包打包时做额外的配置，如果不做额外的配置可不选这个选项（但不保证打包成功）

第三步需要在src/main目录下，新建一个resources目录，将MANIFEST.MF文件保存在这里面，因为如果用默认缺省值的话，在IDEA12版本下会有bug。

点击OK之后，出现如下图界面，右键点击<output root>，点击Create Directory,创建一个libs，将所有的第三方JAR放进libs目录下。

成功之后，如下图所示：

放入之后，点击...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
