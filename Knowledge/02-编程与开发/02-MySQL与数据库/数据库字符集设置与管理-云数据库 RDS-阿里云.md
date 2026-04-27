---
url: https://help.aliyun.com/document_detail/41706.html
source: help.aliyun.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 1941
status: 已处理
---

# 数据库字符集设置与管理-云数据库 RDS-阿里云

> source: [help.aliyun.com](https://help.aliyun.com/document_detail/41706.html)

## 核心要点

1. 官方文档

输入文档关键字查找

字符序命名规则
以字符序对应的字符集名称开头，以 _ci（大小写不敏感）、_cs（大小写敏感）、_bin（按编码值比较，大小写敏感）结尾
2. 例如：当会话的collation_connection设置为字符序utf8_general_ci时，字符a和字符A是等价的；而当其设置为utf8_bin 时，字符a和字符A是不等价的
3. 在可修改参数页签下查找到character_set_server，单击右侧进行修改并单击确定

## 内容预览

官方文档

输入文档关键字查找

字符序命名规则
以字符序对应的字符集名称开头，以 _ci（大小写不敏感）、_cs（大小写敏感）、_bin（按编码值比较，大小写敏感）结尾。
例如：当会话的collation_connection设置为字符序utf8_general_ci时，字符a和字符A是等价的；而当其设置为utf8_bin 时，字符a和字符A是不等价的。
请参考以下示例：

字符集相关 MySQL 命令
show global variables like '%char%'; #查看RDS实例字符集相关参数设置
show global variables like 'coll%'; #查看当前会话字符序相关参数设置
show character set; #查看实例支持的字符集
show collation; #查看实例支持的字符序
show create table table_name \G #查看表字符集设置
show create database database_name \G #查看数据库字符集设置
show create procedure procedure_name \G #查看存储过程字符集设置
show procedure status \G #查看存储过程字符集设置
alter database db_name default charset utf8;...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
