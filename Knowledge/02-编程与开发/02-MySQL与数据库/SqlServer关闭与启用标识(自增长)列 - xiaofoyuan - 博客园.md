---
url: http://www.cnblogs.com/xiaofoyuan/p/3711102.html
source: www.cnblogs.com
date_saved: 2026-04-23

content_type: article
word_count: 1216
status: 已处理
---

# SqlServer关闭与启用标识(自增长)列 - xiaofoyuan - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/xiaofoyuan/p/3711102.html)

## 核心要点

1. 1 --添加新列 2 ALTER TABLE TABLENAME ADD ID int 3 --赋值 4 UPDATE TABLENAME SET ID = IDENTITY_ID 5 --删除标识列 6 ALTER TABLE TABLENAME DROP COLUMN IDENTITY_ID
2. 2 ALTER TABLE TABLENAME ADD ID int
3. 4 UPDATE TABLENAME SET ID = IDENTITY_ID
4. 6 ALTER TABLE TABLENAME DROP COLUMN IDENTITY_ID
5. 显示值插入(修改会话中的IDENTITY_INSERT ),临时性 ,不彻底该表列性质
## 内容预览

笑佛缘

笑佛缘-世界的奇迹

博客园

首页

新随笔

联系

管理

订阅

SqlServer关闭与启用标识(自增长)列

1 --添加新列 2 ALTER TABLE TABLENAME ADD ID int 3 --赋值 4 UPDATE TABLENAME SET ID = IDENTITY_ID 5 --删除标识列 6 ALTER TABLE TABLENAME DROP COLUMN IDENTITY_ID

一般来说大概有2种较好的方案.

1.通过添加列来替换标识列

替换法
1 --添加新列
2 ALTER TABLE TABLENAME ADD ID int
3 --赋值
4 UPDATE TABLENAME SET ID = IDENTITY_ID
5 --删除标识列
6 ALTER TABLE TABLENAME DROP COLUMN IDENTITY_ID

2.显示值插入(修改会话中的IDENTITY_INSERT ),临时性 ,不彻底该表列性质

SET IDENTITY_INSERT [ database_name . [ schema_name ] . ] table { ON | OFF }

显式值插入

1 --一般是组合使用,已确保会话中IDENTITY_INSERT的完整状态
2 SET IDENTITY_INSERT TABLENAME ON --关闭
3 INSERT INTO TABLENAME(IDENTYTY_ID,...) VALUES(...)
4 INSERT INTO TABLENAME(IDENTYTY_ID,...) VALUES(...)
5 INSERT INTO TABLENAME(IDENTYTY_ID,...) VALUES(...)
6 SET IDENTITY_INSERT test OFF --开启

关于这种方式,需要注意如下:

A.任何时候，一个会话中只有一个表的 IDENTITY_INSERT 属性可以设置为 ON ,想修改其他表,必须将前一个ON状态改回OFF

B.如果插入值大于表的当前标识值，则 SQL Server 自动将新插入值作为当前标识值使用

C.SET IDENTITY_INSERT 的设置是在执行或运行时设置的

当然还有其他的方案,比如通过系统存储过程sp_configure 修改列的属性. 其中选择看场景吧

内容来源：http://www.5aspx.com/sql/2012/5252.html

posted @
2014-05-06 11:17
xiaofoyuan
阅读(28558)
评论(0)

收藏
举报

刷新页面返回顶部

公告

博客园
&copy; 2004-2026

浙公网安备 33010602011771号
浙ICP备2021040463号-3
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
