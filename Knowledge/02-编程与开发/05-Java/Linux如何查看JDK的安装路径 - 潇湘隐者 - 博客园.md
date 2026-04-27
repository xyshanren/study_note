---
url: https://www.cnblogs.com/kerrycode/archive/2015/08/27/4762921.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端, DevOps
content_type: article
word_count: 5244
status: 已处理
---

# Linux如何查看JDK的安装路径 - 潇湘隐者 - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/kerrycode/archive/2015/08/27/4762921.html)

## 核心要点

1. 使用$JAVA_HOME的话能定位JDK的安装路径的前提是配置了环境变量$JAVA_HOME，否则如下所示，根本定位不到JDK的安装路径
2. [root@localhost ~]# java -version
3. OpenJDK Runtime Environment (rhel-2.
4. OpenJDK 64-Bit Server VM (build 24.
5. [root@localhost ~]# echo $JAVA_HOME
## 内容预览

代码改变世界

Cnblogs

Dashboard

Login

Home

Contact

Gallery

Subscribe

RSS

潇湘隐者

Linux如何查看JDK的安装路径

2015-08-27 12:14
潇湘隐者
阅读(199286)
评论(5)

收藏
举报

如何在一台Linux服务器上查找JDK的安装路径呢？ 有那些方法可以查找定位JDK的安装路径？是否有一些局限性呢？ 下面总结了一下如何查找JDK安装路径的方法.

1：echo $JAVA_HOME
使用$JAVA_HOME的话能定位JDK的安装路径的前提是配置了环境变量$JAVA_HOME，否则如下所示，根本定位不到JDK的安装路径
[root@localhost ~]# java -version
java version "1.7.0_65"
OpenJDK Runtime Environment (rhel-2.5.1.2.el6_5-x86_64 u65-b17)
OpenJDK 64-Bit Server VM (build 24.65-b04, mixed mode)
[root@localhost ~]# echo $JAVA_HOME

2：which java
首先要申明一下which java是定位不到安装路径的。which java定位到的是java程序的执行路径。网上的资料都是人云亦云，完全不去思考。那么怎么定位到java的安装路径呢？下面我们来看看例子吧,如下所示：
[root@localhost ~]# java -version java version "1.7.0_65" OpenJDK Runtime Environment (rhel-2.5.1.2.el6_5-x86_64 u65-b17) OpenJDK 64-Bit Server VM (build 24.65-b04, mixed mode) [root@localhost ~]# which java /usr/bin/java [root@localhost ~]# ls -lrt /usr/bin/java lrwxrwxrwx. 1 root root 22 Aug 17 15:12 /usr/bin/java -> /etc/alternatives/java [root@localhost ~]# ls -lrt /etc/alternatives/java lrwxrwxrwx. 1 root root 46 Aug 17 15:12 /etc/alternatives/java -> /usr/lib/jvm/jre-1.7.0-openjdk.x86_64/bin/java [root@localhost ~]# [root@localhost ~]# cd /usr/lib/jvm [root@localhost jvm]# ls java-1.6.0-openjdk-1.6.0.0.x86_64 java-1.7.0-openjdk-1.7.0.65.x86_64 jre jre-1.6.0 jre-1.6.0-openjdk.x86_64 jre-1.7.0 jre-1.7.0-openjdk.x86_64 jre-openjdk [root@localhost jvm]#

whereis java 也是如此，它本身不能定位到安装路径。可以通过上面例子去定位安装路径

3： rpm -ql packagename

如果JDK是源码安...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
