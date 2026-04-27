---
url: https://www.cnblogs.com/qqhexi/p/7243073.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 1736
status: 已处理
---

# mysql如何正则匹配查询 - 黑猫love - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/qqhexi/p/7243073.html)

## 核心要点

1. select * from info where name regexp '^L';
2. eg1: 从info表name字段中查询以L开头y结尾中间有两个任意字符的记录
3. eg1: 从info表name字段中查询包含c、e、o三个字母中任意一个的记录
4. eg3: 从info表name字段中查询包含数字或a、b、c三个字母中任意一个的记录
5. eg1: 从info表name字段中查询包含a-w字母和数字以外字符的记录
## 内容预览

黑猫love

博客园

首页

新随笔

联系

管理

订阅

mysql如何正则匹配查询

---恢复内容开始---

基本形式

属性名 regexp ‘匹配方式'

正则表达式的模式字符

^ 匹配字符开始的部分

eg1: 从info表name字段中查询以L开头的记录

select * from info where name regexp '^L';

eg2: 从info表name字段中查询以aaa开头的记录

select * from info where name regexp '^aaa';

$ 匹配字符结束的部分

eg1: 从info表name字段中查询以c结尾的记录

select * from info where name regexp 'c$';

eg2: 从info表name字段中查询以aaa结尾的记录

select * from info where name regexp 'aaa$';

. 匹配字符串中的任意一个字符，包括回车和换行

eg1: 从info表name字段中查询以L开头y结尾中间有两个任意字符的记录

select * from info where name regexp '^L..y$';

[字符集合]匹配字符集合中的任意字符

eg1: 从info表name字段中查询包含c、e、o三个字母中任意一个的记录

select * from info where name regexp '[ceo]';

eg2: 从info表name字段中查询包含数字的记录

select * from info where name regexp '[0-9]';

eg3: 从info表name字段中查询包含数字或a、b、c三个字母中任意一个的记录

select * from info where name regexp '[0-9a-c]';

[^字符集合]匹配除了字符集合外的任意字符

eg1: 从info表name字段中查询包含a-w字母和数字以外字符的记录

select * from info where name regexp '[^a-w0-9]';

s1|s2|s3 匹配s1s2s3中的任意一个

eg1: 从info表name字段中查询包含'ic'的记录

select * from info where name regexp 'ic';

eg2: 从info表name字段中查询包含ic、uc、ab三个字符串中任意一个的记录

select * from info where name regexp 'ic|uc|ab';

* 代表多个该字符前的字符，包括0个或1个

eg1: 从info表name字段中查询c之前出现过a的记录

select * from info where name regexp 'a*c';

+ 代表多个该字符前的字符，包括1个

eg1: 从info表name字段中查询c之前出现过a的记录

select * from info where name regexp 'a+c';(注意比较结果！)

字符串{N} 字符串出现N次

eg1: 从info表name字段中查询出现过a3次的记录

select * from　info where name regexp 'a{3}';

字符串{M，N}字符串最少出现M次，最多出现N次

eg1: 从info表name字段中查询ab出现最少1次最多3次的记录

select * fro...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
