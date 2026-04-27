---
url: https://cloud.tencent.com/developer/article/1791289
source: cloud.tencent.com
date_saved: 2026-04-23
tags: LLM, 前端, 后端
content_type: article
word_count: 8000
status: 已处理
---

# MySQL8.0 配置文件详细说明-腾讯云开发者社区-腾讯云

> source: [cloud.tencent.com](https://cloud.tencent.com/developer/article/1791289)

## 核心要点

1. # 默认情况下，socket文件应为/usr/local/mysql/mysql.
2. socket,所以可以ln -s xx /tmp/mysql.
3. ##########################################################################################################
4. # 临时目录 比如load data infile会用到,一般都是使用/tmp
5. # 事务隔离级别，默认为可重复读（REPEATABLE-READ）。
## 内容预览

JavaEdge

MySQL8.0 配置文件详细说明

关注作者

腾讯云
开发者社区

文档建议反馈控制台登录/注册

首页学习
活动
专区
圈层
工具

MCP广场

文章/答案/技术大牛搜索
搜索关闭

发布

JavaEdge

社区首页 >专栏 >MySQL8.0 配置文件详细说明

MySQL8.0 配置文件详细说明

JavaEdge

关注

发布于 2021-02-23 11:59:25
发布于 2021-02-23 11:59:25
6K0

举报

文章被收录于专栏：JavaEdgeJavaEdge

代码语言：javascript

复制

# 客户端设置
[client]
port = 3306
# 默认情况下，socket文件应为/usr/local/mysql/mysql.socket,所以可以ln -s xx /tmp/mysql.sock
socket = /tmp/mysql.sock

# 服务端设置
[mysqld]

##########################################################################################################
# 基础信息
#Mysql服务的唯一编号 每个mysql服务Id需唯一
server-id = 1

#服务端口号 默认3306
port = 3306

# 启动mysql服务进程的用户
user = mysql

##########################################################################################################
# 安装目录相关
# mysql安装根目录
basedir = /usr/local/mysql-5.7.21

# mysql数据文件所在位置
datadir = /usr/local/mysql-5.7.21/data

# 临时目录 比如load data infile会用到,一般都是使用/tmp
tmpdir = /tmp

# 设置socke文件地址
socket = /tmp/mysql.sock

##########################################################################################################
# 事务隔离级别，默认为可重复读（REPEATABLE-READ）。（此级别下可能参数很多间隙锁，影响性能，但是修改又影响主从复制及灾难恢复，建议还是修改代码逻辑吧）
# 隔离级别可选项目：READ-UNCOMMITTED READ-COMMITTED REPEATABLE-READ SERIALIZABLE
# transaction_isolation = READ-COMMITTED
transaction_isolation = REPEATABLE-READ

##########################################################################################################
# 数据库引擎与字符集相关设置

# mysql 5.1 之后，默认引擎就是InnoDB了
default_storage_engine = InnoDB
# ...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
