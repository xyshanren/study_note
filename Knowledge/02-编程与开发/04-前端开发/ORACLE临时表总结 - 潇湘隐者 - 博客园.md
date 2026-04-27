---
url: http://www.cnblogs.com/kerrycode/p/3285936.html
source: www.cnblogs.com
date_saved: 2026-04-23

content_type: article
word_count: 6000
status: 已处理
---

# ORACLE临时表总结 - 潇湘隐者 - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/kerrycode/p/3285936.html)

## 核心要点

1.  

临时表语法

 

 

 

临时表分类

 

ORACLE临时表有两种类型：会话级的临时表和事务级的临时表
2. 1）ON COMMIT DELETE ROWS

它是临时表的默认参数，表示临时表中的数据仅在事物过程（Transaction）中有效，当事物提交（COMMIT）后，临时表的暂时段将被自动截断（TRUNCATE），但是临时表的结构 以及元数据还存储在用户的数据字典中。如果临时表完成它的使命后，最好删除临时表，否则数据库会残留很多临时表的表结构和元数据
3. 2）ON COMMIT PRESERVE ROWS

它表示临时表的内容可以跨事物而存在，不过，当该会话结束时，临时表的暂时段将随着会话的结束而被丢弃，临时表中的数据自然也就随之丢弃。但是临时表的结构以及元数据还存储在用户的数据字典中。如果临时表完成它的使命后，最好删除临时表，否则数据库会残留很多临时表的表结构和元数据

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

 ORACLE临时表总结

2013-08-27 20:23 
潇湘隐者 
阅读(127551) 
评论(11) 
 
收藏 
举报

临时表概念

   临时表就是用来暂时保存临时数据（亦或叫中间数据）的一个数据库对象，它和普通表有些类似，然而又有很大区别。它只能存储在临时表空间，而非用户的表空间。ORACLE临时表是会话或事务级别的，只对当前会话或事务可见。每个会话只能查看和修改自己的数据。 

 

临时表语法

 

 

 

临时表分类

 

ORACLE临时表有两种类型：会话级的临时表和事务级的临时表。

1）ON COMMIT DELETE ROWS

它是临时表的默认参数，表示临时表中的数据仅在事物过程（Transaction）中有效，当事物提交（COMMIT）后，临时表的暂时段将被自动截断（TRUNCATE），但是临时表的结构 以及元数据还存储在用户的数据字典中。如果临时表完成它的使命后，最好删除临时表，否则数据库会残留很多临时表的表结构和元数据。

2）...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
