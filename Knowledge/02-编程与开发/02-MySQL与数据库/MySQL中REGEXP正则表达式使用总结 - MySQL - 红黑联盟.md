---
url: https://www.2cto.com/database/201803/733201.html
source: www.2cto.com
date_saved: 2026-04-23
tags: 数据库
content_type: article
word_count: 5756
status: 已处理
---

# MySQL中REGEXP正则表达式使用总结 - MySQL - 红黑联盟

> source: [www.2cto.com](https://www.2cto.com/database/201803/733201.html)

## 核心要点

1. MySQL中REGEXP正则表达式使用总结

 有天在项目中,需要用到根据列值来匹配数据来源问题，如当改列值为数值时,为字符串时...通过上网搜索发现,mysql竟然支持正则表达式,

下面文章,转自网上其他人得,供自己快速查找和他人参考
2. MySQL采用Henry Spencer的正则表达式实施，其目标是符合POSIX 1003.2。请参见附录C：感谢。MySQL采用了扩展的版本，以支持在SQL语句中与REGEXP操作符一起使用的模式匹配操作。请参见3.3.4.7节，&ldquo;模式匹配&rdquo;
3. 在本附录中，归纳了在MySQL中可用于REGEXP操作的特殊字符和结构，并给出了一些示例。本附录未包含可在Henry Spencer的regex(7)手册页面中发现的所有细节。该手册页面包含在MySQL源码分发版中，位于regex目录下的regex.7文件中

## 内容预览

MySQL中REGEXP正则表达式使用总结

 有天在项目中,需要用到根据列值来匹配数据来源问题，如当改列值为数值时,为字符串时...通过上网搜索发现,mysql竟然支持正则表达式,

下面文章,转自网上其他人得,供自己快速查找和他人参考。

MySQL采用Henry Spencer的正则表达式实施，其目标是符合POSIX 1003.2。请参见附录C：感谢。MySQL采用了扩展的版本，以支持在SQL语句中与REGEXP操作符一起使用的模式匹配操作。请参见3.3.4.7节，&ldquo;模式匹配&rdquo;。

在本附录中，归纳了在MySQL中可用于REGEXP操作的特殊字符和结构，并给出了一些示例。本附录未包含可在Henry Spencer的regex(7)手册页面中发现的所有细节。该手册页面包含在MySQL源码分发版中，位于regex目录下的regex.7文件中。

正则表达式描述了一组字符串。最简单的正则表达式是不含任何特殊字符的正则表达式。例如，正则表达式hello匹配hello。

非平凡的正则表达式采用了特殊的特定结构，从而使得它们能够与1个以上的字符串匹配。例如，正则表达式hello|word匹配字符串hello或字符串word。

作为一个更为复杂的示例，正则表达式B[an]*s匹配下述字符串中的任何一个：Bananas，Baaaaas，Bs，以及以B开始、以s结束...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
