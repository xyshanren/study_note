---
url: http://dblab.xmu.edu.cn/blog/818-2/
source: dblab.xmu.edu.cn
date_saved: 2026-04-23

content_type: article
word_count: 755
status: 已处理
---

# 解决Hadoop启动时，没有启动datanode_厦大数据库实验室博客

> source: [dblab.xmu.edu.cn](http://dblab.xmu.edu.cn/blog/818-2/)

## 核心要点

1. 厦大数据库实验室博客

 总结、分享、收获实验室主页

 搜索：

 ﻿

 Hadoop在多次运行下列指令：

hadoop namenode -format
sbin/start-dfs.sh

经常会出现没有启动datanode的情况
2. sbin/stop-dfs.sh

下次想重新运行Hadoop，不用再格式化namenode,直接启动Hadoop即可

sbin/start-dfs.sh

 &copy; 2014 厦大数据库实验室

## 内容预览

厦大数据库实验室博客

 总结、分享、收获实验室主页

 搜索：

 ﻿

 Hadoop在多次运行下列指令：

hadoop namenode -format
sbin/start-dfs.sh

经常会出现没有启动datanode的情况。

运行命令:

jps

发现没有datanode线程。

现给出原因和解决方案

原因

当我们使用hadoop namenode -format格式化namenode时，会在namenode数据文件夹（这个文件夹为自己配置文件中dfs.name.dir的路径）中保存一个current/VERSION文件，记录clusterID，datanode中保存的current/VERSION文件中的clustreID的值是上一次格式化保存的clusterID，这样，datanode和namenode之间的ID不一致。

解决方法

第一种：如果dfs文件夹中没有重要的数据，那么删除dfs文件夹，再重新运行下列指令：
hadoop namenode -format
sbin/start-dfs.sh

第二种:如果dfs文件中有重要的数据，那么在dfs/name目录下找到一个current/VERSION文件，记录clusterID并复制。然后dfs/data目录下找到一个current/VERSION文件，将其中clustreID的值替换成刚刚...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
