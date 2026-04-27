---
url: http://blog.csdn.net/dandandeshangni/article/details/44057871###;
source: blog.csdn.net
date_saved: 2026-04-23
tags: Python, 后端, 数据库
content_type: article
word_count: 6000
status: 已处理
---

# idea 修改jsp页面需要重新部署项目的额问题_idea设置jsp修改后代码自己重新打包-CSDN博客

> source: [blog.csdn.net](http://blog.csdn.net/dandandeshangni/article/details/44057871###;)

## 核心要点

1. 福利倒计时

 :

 :

 立减 ¥

 普通VIP年卡可用

 立即使用

 java干货

 关注
 关注

 8

 点赞

 踩

 10

 收藏

 觉得还不错
2. 4 条评论
 您还未登录，请先
 登录
 后发表或查看评论

 IntelliJ IDEA 2017配置JSP+tomcat

 10-30

 1
3. 简单测试JSP：在配置好Tomcat和创建Web项目后，可以在本地机器上通过Tomcat服务器运行JSP页面，以检查是否能够正确显示

## 内容预览

idea 修改jsp页面需要重新部署项目的额问题

 最新推荐文章于 2024-06-02 16:29:34 发布

 翻译

 最新推荐文章于 2024-06-02 16:29:34 发布
 ·
 1.7w 阅读

 ·

 8

 ·

 10

 文章标签：

 #IDE idea

 JAVAweb
 专栏收录该内容

 163 篇文章

 订阅专栏

 本文详细介绍了在使用IntelliJ IDEA开发时遇到的JSP文件改动后，Tomcat中未能立即响应的问题，并提供了通过修改服务器配置来解决此问题的具体步骤。通过在服务器配置中选择'onframedeactivation'为'updateclassesandresources'，并使用'Redeploy'作为'onupdateaction'，可以确保JSP文件改动后能立即在Tomcat中生效。同时，文章还指出服务器的Artifact类型会影响配置选项，选择'warexplored'类型会显示'updateclassesandresources'选项。

主要的看 on update action 和 on frame deactivation 

具体解释如下

intellij idea默认文件是自动保存的但是手头有个项目jsp文件改动后在tomc...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
