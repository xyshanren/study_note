---
url: https://www.cnblogs.com/wangmo/p/7737109.html
source: www.cnblogs.com
date_saved: 2026-04-23

content_type: article
word_count: 1301
status: 已处理
---

# git设置忽略文件和目录 - wangmo - 博客园

> source: [www.cnblogs.com](https://www.cnblogs.com/wangmo/p/7737109.html)

## 核心要点

1. .gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的
2.  正确的做法是在每个clone下来的仓库中手动设置不要检查特定文件的更改情况
3.  git update-index --assume-unchanged PATH    在PATH处输入要忽略的文件

## 内容预览

wangmo

跨过这道坎

博客园

首页

新随笔

联系

订阅

管理

 git设置忽略文件和目录

 

1.登录gitbash命令端进入本地git库目录

Administrator@PC201601200946 MINGW32 /d/gitrespository/crmweb (master)

2.创建.gitignore

3.修改文件，添加忽略正则

　　.idea //忽略.idea文件夹及文件夹下文件

　　*.iml //忽略以.iml结尾的文件

【例子】

# 忽略*.o和*.a文件

 *.[oa]

# 忽略*.b和*.B文件，my.b除外

*.[bB]

!my.b

# 忽略dbg文件和dbg目录

dbg

# 只忽略dbg目录，不忽略dbg文件

dbg/

# 只忽略dbg文件，不忽略dbg目录

dbg

!dbg/

# 只忽略当前目录下的dbg文件和目录，子目录的dbg不在忽略范围内

/dbg

# 以'#'开始的行，被视为注释.

 * ？：代表任意的一个字符
    * ＊：代表任意数目的字符
    * {!ab}：必须不是此类型
    * {ab,bb,cx}：代表ab,bb,cx中任一类型即可
&...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
