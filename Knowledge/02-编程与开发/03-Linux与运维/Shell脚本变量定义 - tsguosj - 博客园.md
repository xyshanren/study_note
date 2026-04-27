---
url: https://www.cnblogs.com/guosj/p/4568307.html
source: www.cnblogs.com
date_saved: 2026-04-23
tags: 后端
content_type: article
word_count: 1599
status: 已处理
---

# Shell脚本变量定义 - tsguosj - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/guosj/p/4568307.html)

## 核心要点

1. 定义变量

  定义变量时，变量名不加美元符号（$），如：

  代码如下:

  variableName="value"

   注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：

•首个字符必须为字母（a-z，A-Z）
2. •不能使用bash里的关键字（可用help命令查看保留关键字）
3. 重新定义变量

已定义的变量，可以被重新定义，如： 

代码如下:

your_name="tom"
echo $your_name
 

your_name="alibaba"
echo $your_name

 这样写是合法的，但注意，第二次赋值的时候不能写$your_name="alibaba"，使用变量的时候才加美元符（$）

## 内容预览

tsguosj

博客园

首页

新随笔

联系

订阅

管理

 Shell脚本变量定义

http://blog.csdn.net/qyf_5445/article/details/8886071

自定义变量
bash中变量无类型区分
aa=abc123    定义变量并赋值abc123
aa=          定义空变量或者清空变量aa,但变量还存在
export test="hello world"  设定环境变量test
export或者export -p        显示所有环境变量
declare / typeset 选项 变量名
declare 或 typeset 有同样的功能:指定变量属性。如果使用 declare 后面并没有接任何参数,那么 bash 就会主动的将所有的变量名称与内容通通叫出来,就好像使用 set 一样！ 
选项：
-a 将后面的变量定义成为数组 (array)
-i 将后面的变量定义成为整数(integer)
-x 将后面的变量变成环境变量,同export 一样,
-r 将后面的变量设定为只读 ,该变量不可被更改内容,也不能 unset
-f 列出脚本中的函数
readonly用来设置只读变量
readon...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
