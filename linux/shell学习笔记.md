---
title: shell学习笔记
author:
  - 守一
created: 2025-03-13
description: shell语言的学习笔记
tags:
  - shell
  - Linux
---
## bash解释器的特性

### history

- 关于history

```bash
# 查看执行的历史命令
history

# 设置记录历史命令条数的参数
echo $HISTSIZE

# 默认历史命令存储文件为：~/.bash_history
# 设置历史命令保存文件的参数
echo $HISTFILE
```

- history命令用法
参数：-c 清除历史命令列表，-r 读取历史文件恢复历史命令列表

- 快速执行历史命令

```bash
# !历史id，快速执行历史命令
!1004
echo $HISTSIZE
1000

# !!，执行上次的命令（也可以通过上下箭头键找到最近执行的命令）
!!
echo $HISTSIZE
1000
```

### 总结
- 文件路径tab键补全。
- 命令补全。
- 快捷键：ctrl + a,e,u,k,l。
- 通配符。
- 命令历史。
- 命令别名。
- 命令行展开。

## 变量
### 变量定义与赋值
变量类型，bash是弱类型，无需事先声明类型，是将声明和赋值同时进行，默认将所有变量都认为是字符串。
**变量赋值时不得有空格**
```bash
name="abc"
```

