---
url: http://www.linuxidc.com/Linux/2015-02/113427.htm
source: www.linuxidc.com
date_saved: 2026-04-23
tags: 数据库, DevOps, 学习-教程
content_type: article
word_count: 2313
status: 已处理
---

# Oracle使用临时变量_数据库技术_Linux公社-Linux系统门户网站

> source: [www.linuxidc.com](http://www.linuxidc.com/Linux/2015-02/113427.htm)

## 核心要点

1. 在Oracle数据库中，可以使用变量来编写通用的sql语句，在运行sql语句时，为变量输入值，就会在sql语句中将变量替换成这些值。
2. 临时变量只在使用它的sql语句中有效，变量值不能保留，临时变量也称为替换变量。
3. 在sql语句中，如果在某个变量前面使用了&符号，那么久表示该变量是一个临时变量，执行sql语句时，系统会提示用户为该变量提供一个具体的数据。
4. SQL> select * from dept where deptno>&temp;
5. 原值 1: select * from dept where deptno>&temp
## 内容预览

你好，游客 登录

注册

搜索

首页Linux新闻Linux教程数据库技术Linux编程服务器应用Linux安全Linux下载Linux认证Linux主题Linux壁纸Linux软件数码手机电脑

首页 → 数据库技术

背景：

阅读新闻

Oracle使用临时变量

[日期：2015-02-14]

来源：Linux社区 

作者：理央silence

[字体：大 中 小]

在Oracle数据库中，可以使用变量来编写通用的sql语句，在运行sql语句时，为变量输入值，就会在sql语句中将变量替换成这些值。

临时变量只在使用它的sql语句中有效，变量值不能保留，临时变量也称为替换变量。在sql语句中，如果在某个变量前面使用了&符号，那么久表示该变量是一个临时变量，执行sql语句时，系统会提示用户为该变量提供一个具体的数据。

例如，在sql*plus中执行以下的命令：
SQL> select * from dept where deptno>&temp;
输入 temp 的值: 30
原值 1: select * from dept where deptno>&temp
新值 1: select * from dept where deptno>30
DEPTNO DNAME LOC
---------- -------------- -------------
40 OPERATIONS BOSTON
SQL>
也可以使用多个的临时变量，事例如下：

SQL> select &column_name,dname,loc from dept where &column_name>20;
输入 column_name 的值: deptno
输入 column_name 的值: deptno
原值 1: select &column_name,dname,loc from dept where &column_name>20
新值 1: select deptno,dname,loc from dept where deptno>20

DEPTNO DNAME LOC
---------- -------------- -------------
30 SALES CHICAGO
40 OPERATIONS BOSTON
在sql语句中，如果希望重新使用某个变量并且不希望重新提示输入，可以使用&&符号来定义临时变量。如下：
SQL> select &&column_name,dname,loc from dept where &&column_name>10;
输入 column_name 的值: deptno
原值 1: select &&column_name,dname,loc from dept where &&column_name>10
新值 1: select deptno,dname,loc from dept where deptno>10

DEPTNO DNAME LOC
---------- -------------- -------------
20 RESEARCH DALLAS
30 SALES CHICAGO
40 OPERATIONS BOSTON

更多Oracle相关信息见Oracle 专题页面 http://www.linuxidc.com/topicnews.aspx?tid=12

本文永久更新链接地址：http://www.linuxidc.com/Linux/2015-02/1...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
