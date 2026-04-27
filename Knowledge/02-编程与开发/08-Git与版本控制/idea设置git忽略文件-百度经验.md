---
url: https://jingyan.baidu.com/article/3a2f7c2ef5b2c926afd6111b.html
source: jingyan.baidu.com
date_saved: 2026-04-23
tags: 后端, 学习-教程, 效率工具
content_type: article
word_count: 1619
status: 已处理
---

# idea设置git忽略文件-百度经验

> source: [jingyan.baidu.com](https://jingyan.baidu.com/article/3a2f7c2ef5b2c926afd6111b.html)

## 核心要点

1. 2
接下来我们在右侧会看到ignore files and folders，我们可以看到在下方的框里已经有很多文件名或文件后缀了，而且每个用英文格式下的;隔开，因此我们加入 *.iml;.idea;target; 然后点OK进行保存
2. 3
还有，如果你当前打开的项目已经从远程仓库拉取过代码的话，或者是已经配置过git的总分支的情况下，还有其他设置忽略单个文件的方法。例如我们在右下角可以看到git字样，显示当前的分支，我们点击该分支，会出现设置页面，如下图所示
3. 4
我们在此页面空白的地方，点击鼠标右键，会看到Now ChangeList，前面有一个+号，我们点击该+，在弹出的框里随便输入名字即可

## 内容预览

百度经验 > 游戏/数码 > 互联网

idea设置git忽略文件

原创
|
浏览：80129
|
更新：2019-06-07 09:26
|
标签：GIT 

1

2

3

4

5

6

7

分步阅读

在使用IntelliJ IDEA这个集成开发工具进行开发时，我们会用到git等代码版本管理工具将代码push到远程仓库，IntelliJ IDEA有自带的git，当然我们也可以自己安装好了，在IntelliJ IDEA中进行配置。git本身是支持通过编辑.gitignore文件去忽略一些不想提交的文件的，如果项目工程代码中没有编辑.gitignore文件，那么IntelliJ IDEA会默认把当前本地和远程不一致的文件都列出来，事实上像后缀名是.iml， .idea，target 等文件是不需要提交的，如果每次都手动忽略的话会非常辛苦，而且可能不小心给提交了，回滚也麻烦，因此小编为大家介绍idea设置git忽略文件的方法，下面跟着小编一起学习吧。

工具/原料
IntelliJ IDEA

方法/步骤
1
IntelliJ IDEA设置文件自动忽略后，以后提交代码就会自动忽略.iml， .idea，target 文件夹等这些文件，该设置仅需设置一次即可，以后无论多少个项目都可以自动忽略该项目下面的...

## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
