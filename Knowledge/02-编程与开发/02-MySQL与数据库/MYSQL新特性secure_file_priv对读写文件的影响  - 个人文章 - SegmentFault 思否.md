---
url: https://segmentfault.com/a/1190000009333563
source: segmentfault.com
date_saved: 2026-04-23
tags: 数据库, DevOps
content_type: article
word_count: 1414
status: 已处理
---

# MYSQL新特性secure_file_priv对读写文件的影响  - 个人文章 - SegmentFault 思否

> source: [segmentfault.com](https://segmentfault.com/a/1190000009333563)

## 核心要点

1. MYSQL新特性secure_file_priv对读写文件的影响
2. 1290 - The MySQL server is running with the --secure-file-priv option so it cannot execute this statement
3. secure-file-priv参数是用来限制LOAD DATA, SELECT .
4. OUTFILE, and LOAD_FILE()传到哪个指定目录的。
5. ure_file_priv的值为null ，表示限制mysqld 不允许导入|导出
## 内容预览

MYSQL新特性secure_file_priv对读写文件的影响

7shad0w

2017-05-08 阅读 1 分钟

1

1290 - The MySQL server is running with the --secure-file-priv option so it cannot execute this statement

secure-file-priv特性
secure-file-priv参数是用来限制LOAD DATA, SELECT ... OUTFILE, and LOAD_FILE()传到哪个指定目录的。

ure_file_priv的值为null ，表示限制mysqld 不允许导入|导出

当secure_file_priv的值为/tmp/ ，表示限制mysqld 的导入|导出只能发生在/tmp/目录下

当secure_file_priv的值没有具体值时，表示不对mysqld 的导入|导出做限制

如何查看secure-file-priv参数的值：

show global variables like '%secure%';

mysql> show global variables like '%secure%';

Variable_name
Value

require_secure_transport
OFF

secure_auth
ON

secure_file_priv
/var/lib/mysql-files/

3 rows in set (0.02 sec)

MYSQL新特性secure_file_priv对读写文件的影响
此开关默认为NULL，即不允许导入导出。

解决问题:
windows下：修改my.ini 在[mysqld]内加入secure_file_priv =

linux下：修改my.cnf 在[mysqld]内加入secure_file_priv =
MYSQL新特性secure_file_priv对读写文件的影响
然后重启mysql，再查询secure_file_priv

mysql

赞1收藏2分享

阅读 46k更新于 2017-05-08
举报

7shad0w
6 声望2 粉丝

关注作者

« 上一篇
phpmyadmin访问显示空白问题
下一篇 »
php利用Libchart库绘图

▲
12

引用和评论
推荐阅读

Http Smuggling Attack - 1
7shad0w阅读 3k

MySQL数据库InnoDB引擎行级锁锁定范围详解
JerryTse赞 21阅读 18k评论 4

centos下升级mysql5.5.47到5.7.14操作过程
少放香菜赞 1阅读 3.6k

mysql数据库无法被其他ip访问的问题
少放香菜阅读 3.4k

Windows Server 2008安装Mysql 5.7的注意事项
少放香菜阅读 4.9k

MySQL 行锁、索引理解及常见面试题
高旭阅读 2.5k

揭秘MySQL索引分类
SevenCoding赞 1阅读 533

0 条评论得票最新

提交评论
评论支持部分 Markdown 语法：**粗体** _斜体_ [链接](http://example.com) `代码` - 列表 > 引用。你还可以使用 @ 来通知其他用户。
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
