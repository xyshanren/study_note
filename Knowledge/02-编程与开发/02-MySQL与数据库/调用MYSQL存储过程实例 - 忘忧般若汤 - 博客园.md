---
url: http://www.cnblogs.com/mitang/p/3537757.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 3137
status: 已处理
---

# 调用MYSQL存储过程实例 - 忘忧般若汤 - 博客园

> source: [www.cnblogs.com](http://www.cnblogs.com/mitang/p/3537757.html)

## 核心要点

1. net/ewing333/article/details/5906887
2. com/kkcheng/archive/2010/03/19/1689672.
3. com/dreamontheway/item/8041f26ad5070131ad3e8346
4. $conn = mysql_connect('localhost','root','root') or die ("数据连接错误!
5. INSERT INTO user (id, username, sex) VALUES (NULL, 's', '0');
## 内容预览

忘忧般若汤

好师言 , 仗剑走天涯，偶尔喝喝酒。

博客园

首页

新随笔

联系

订阅

管理

调用MYSQL存储过程实例

PHP调用MYSQL存储过程实例

http://blog.csdn.net/ewing333/article/details/5906887

http://www.cnblogs.com/kkcheng/archive/2010/03/19/1689672.html

http://hi.baidu.com/dreamontheway/item/8041f26ad5070131ad3e8346

实例一：无参的存储过程

$conn = mysql_connect('localhost','root','root') or die ("数据连接错误!!!");
mysql_select_db('test',$conn);
$sql = "
create procedure myproce()
begin
INSERT INTO user (id, username, sex) VALUES (NULL, 's', '0');
end;
";
mysql_query($sql);//创建一个myproce的存储过程

$sql = "call test.myproce();";
mysql_query($sql);//调用myproce的存储过程，则数据库中将增加一条新记录。

实例二：传入参数的存储过程

$sql = "
create procedure myproce2(in score int)
begin
if score >= 60 then
select 'pass';
else
select 'no';
end if;
end;
";
mysql_query($sql);//创建一个myproce2的存储过程
$sql = "call test.myproce2(70);";
mysql_query($sql);//调用myproce2的存储过程,看不到效果。

实例三：传出参数的存储过程

$sql = "
create procedure myproce3(out score int)
begin
set score=100;
end;
";
mysql_query($sql);//创建一个myproce3的存储过程
$sql = "call test.myproce3(@score);";
mysql_query($sql);//调用myproce3的存储过程
$result = mysql_query('select @score;');
$array = mysql_fetch_array($result);

echo '';print_r($array);

实例四：传出参数的inout存储过程

$sql = "
create procedure myproce4(inout sexflag int)
begin
SELECT * FROM user WHERE sex = sexflag;
end;
";
mysql_query($sql);//创建一个myproce4的存储过程
$sql = "set @sexflag = 1";
mysql_query($sql);//设置性别参数为1
$sql = "call test.myproce4(@sexflag);";
mysql_query($sql);//调用myproce4的存储过程

实例五：使用变量的存储过程...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
