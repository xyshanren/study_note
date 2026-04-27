---
url: https://www.2cto.com/database/201702/596860.html
source: www.2cto.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 1664
status: 已处理
---

# mysql函数date()，date_format()和str_to_date() - MySQL - 红黑联盟

> source: [www.2cto.com](https://www.2cto.com/database/201702/596860.html)

## 核心要点

1. mysql函数date()，date_format()和str_to_date()
2. 2、str_to_date()函数：按照指定日期或时间显示式 将字符串转换为日期或日期时间类型；
3. 3、date_format()函数：按照指定日期或时间显示式 输出日期或日期时间；
4. SELECT date('2017-02-09 15:25:46.
5. 2、date_format(datestring，format)
## 内容预览

mysql函数date()，date_format()和str_to_date()

定义和用法：

1、DATE() 函数：返回日期或日期时间表达式的日期部分；

2、str_to_date()函数：按照指定日期或时间显示式 将字符串转换为日期或日期时间类型；

3、date_format()函数：按照指定日期或时间显示式 输出日期或日期时间；

实例：

1、date(datestring)

datestring是合法的日期表达式

如：

SELECT date('2017-02-09 15:25:46.635')
FROM dual;
-->'2017-02-09'

2、date_format(datestring，format)

datestring参数是合法的日期。format 规定日期/时间的输出式。

如：

SELECT STR_TO_DATE('2017-02-09 15:25:46.635','%Y-%m-%d')
FROM dual;
-->'2017-02-09'

SELECT STR_TO_DATE('2017-02-09 15:25:46','%Y-%m-%d %H:%i:%s')
FROM DUAL;
-->'2017-02-09 15:25:46'

SELECT STR_TO_DATE('2017-02-09 15:25','%Y-%m-%d %k:%i')
FROM DUAL;
-->'2017-02-09 15:25:00'

3、date_format(datestring，format)

datestring参数是合法的日期。format 规定日期/时间的输出式。

如：

当前时间按月-日
时:分:秒显示：

SELECT DATE_FORMAT(NOW(),'%m-%d %h:%i %p')
FROM dual;
-->'02-09 06:00 PM'

当前时间按 年-月-日 时:分:秒 AM/PM显示：

SELECT DATE_FORMAT(NOW(),'%Y-%m-%d %h:%i:%s %p')
FROM dual;
-->'2017-02-09 06:00:35'

当前时间按 年 周 日 时:分:秒显示：

SELECT DATE_FORMAT(NOW(),'%Y %b %d %T')
FROM dual;
-->'2017 Feb 09 18:04:13'

可以使用的式有：

式

描述

%a

缩写星期名

%b

缩写月名

%c

月，数

%D

带有英文前缀的月中的天

%d

月的天，数(00-31)

%e

月的天，数(0-31)

%f

微秒

%H

小时 (00-23)

%h

小时 (01-12)

%I

小时 (01-12)

%i

分钟，数(00-59)

%j

年的天 (001-366)

%k

小时 (0-23)

%l

小时 (1-12)

%M

月名

%m

月，数(00-12)

%p

AM 或 PM

%r

时间，12-小时（hh:mm:ss AM 或 PM）

%S

秒(00-59)

%s

秒(00-59)

%T

时间, 24-小时 (hh:mm:ss)

%U

周 (00-53) 星期日是一周的第一天

%u

周 (00-53) 星期一是一周的第一天

%V

周 (01-53) 星期日是一周的第一天，与 %X 使用

%v

周 (01-53) 星期一是一周的第一天，与 %x 使用

%W...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
