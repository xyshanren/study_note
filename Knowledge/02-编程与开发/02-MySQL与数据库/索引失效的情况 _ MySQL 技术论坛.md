---
url: https://learnku.com/articles/38889
source: learnku.com
date_saved: 2026-04-23
tags: 后端, 数据库, 学习-教程
content_type: article
word_count: 4594
status: 已处理
---

# 索引失效的情况 | MySQL 技术论坛

> source: [learnku.com](https://learnku.com/articles/38889)

## 核心要点

1. 索引知识点也巨多，要想掌握透彻，需要逐个知识点一一击破，今天来先来聊聊哪些情况下会导致索引失效。
2. explain select * from user where name = 'zhangsan' and age = 20 and phone = '18730658760' and pos = 'cxy';
3. 和索引顺序无关，MySQL底层的优化器会进行优化，调整索引的顺序
4. 如计算、函数、（自动or手动）类型转换等操作，会导致索引失效从而全表扫描
5. explain select name,age,pos,phone from user where age = 20;
## 内容预览

MySQL

社区

动态

AI作品

话题列表

社区 Wiki

优质外文

招聘求职

MySQL 实战教程

社区文档

登录

注册

微信登录

索引失效的情况

18

37

1

zhangdeTalk 的个人博客

/

14
/
1
/

创建于 6年前

/
更新于 1年前

/
1 个改进

索引对于MySQL而言，是非常重要的篇章。索引知识点也巨多，要想掌握透彻，需要逐个知识点一一击破，今天来先来聊聊哪些情况下会导致索引失效。

图片总结版

全值匹配（索引最佳）

explain select * from user where name = 'zhangsan' and age = 20 and phone = '18730658760' and pos = 'cxy';

和索引顺序无关，MySQL底层的优化器会进行优化，调整索引的顺序
explain select * from user where name = 'zhangsan' and age = 20 and pos = 'cxy' and phone = '18730658760';

1、违反最左前缀法则

如果索引有多列，要遵守最左前缀法则
即查询从索引的最左前列开始并且不跳过索引中的列
explain select * from user where age = 20 and phone = '18730658760' and pos = 'cxy';

2、在索引列上做任何操作

如计算、函数、（自动or手动）类型转换等操作，会导致索引失效从而全表扫描
explain select * from user where left(name,5) = 'zhangsan' and age = 20 and phone = '18730658760';

3、索引范围条件右边的列

索引范围条件右边的索引列会失效
explain select * from user where name = 'zhangsan' and age > 20 and pos = 'cxy';

4、尽量使用覆盖索引

只访问索引查询（索引列和查询列一致），减少select*
explain select name,age,pos,phone from user where age = 20;

5、使用不等于（!=、<>）

mysql在使用不等于（!=、<>）的时候无法使用索引会导致全表扫描（除覆盖索引外）
explain select * from user where age != 20;
explain select * from user where age <> 20;

6、like以通配符开头（’%abc’）

索引失效
explain select * from user where name like '%zhangsan';

索引生效
explain select * from user where name like 'zhangsan%';

7、字符串不加单引号索引失效

explain select * from user where name = 2000;

8、or连接

少用or
explain select * from user where name = '2000' or age = 20 or pos ='cxy';

9、order by

正常（索引参与了排序）
explain select * from user...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
