---
url: https://blog.csdn.net/tao_wei162/article/details/84764871
source: blog.csdn.net
date_saved: 2026-04-23
tags: Python, 后端
content_type: article
word_count: 6000
status: 已处理
---

# 这JDK的配置CLASSPATH中的dt.jar和tools.jar这两个文件-CSDN博客

> source: [blog.csdn.net](https://blog.csdn.net/tao_wei162/article/details/84764871)

## 核心要点

1. 文章标签：

 #java
 #开发工具

 java/guava/python/php/ruby/R/scala/groovy
 专栏收录该内容

 213 篇文章

 订阅专栏

 本文详细解析了JDK配置中dt.jar和tools.jar这两个文件的功能和用途，解释了它们在Java开发过程中的角色，并讨论了在开发过程中不配置这两个包可能带来的影响
2. ——————————————————————————————————————————————————————

答dt.jar和tools.jar是两个java最基本的包里面包含了从java最重要的lang包到各种高级功能如可视化的swing包是java必不可少的
3. rt.jar 默认就在 根classloader的加载路径里面 放在claspath是多此一举  
不信你可以去掉classpath里面的rt.jar  

然后用 java -verbose XXXX 的方式运行一个简单的类 就知道 JVM的系统根Loader的路径里面  

不光rt.jar jre/lib下面的大部分jar 都在这个路径里   

2

## 内容预览

这JDK的配置CLASSPATH中的dt.jar和tools.jar这两个文件

 最新推荐文章于 2024-03-18 22:58:35 发布

 原创

 最新推荐文章于 2024-03-18 22:58:35 发布
 ·
 646 阅读

 ·

 0

 ·

 3

 ·

 CC 4.0 BY-SA版权

 版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

 文章标签：

 #java
 #开发工具

 java/guava/python/php/ruby/R/scala/groovy
 专栏收录该内容

 213 篇文章

 订阅专栏

 本文详细解析了JDK配置中dt.jar和tools.jar这两个文件的功能和用途，解释了它们在Java开发过程中的角色，并讨论了在开发过程中不配置这两个包可能带来的影响。

这JDK的配置CLASSPATH中的dt.jar和tools.jar这两个文件到底是干什么的有人说这个dt.jar是关于swing的 打开这个包确实可以看到和swing有关的类说是如果用到swing就要配置这classpath但是rt.jar中的swing呢 这个不才是真正的包含swing类库吗 还有就...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
