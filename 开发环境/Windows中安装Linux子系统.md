---
title: 在Windows中安装Linux子系统
author:
  - 守一
created: 2019-12-26
description: 在Windows中安装Linux子系统
tags:
  - 虚拟机环境
  - Linux
---

# Windows中安装Linux子系统的详细步骤 #

>早就听说Windows中可以安装Linux子系统，体验了一下，感觉还是不错的，下面直接开始安装和配置步骤吧！

## 开启Windows中的配置 ##

- 首先开启开发者模式  
  打开“所有设置”进入“更新和安全”  
  ![20191226092039.png](https://i.loli.net/2019/12/26/HuwYK1IaJEGd2Mi.png)

  选择左侧的“开发者选项”，然后选择“开发者模式”  
  ![20191226092233.png](https://i.loli.net/2019/12/26/RIGK4pymZB5b8zo.png)

- 然后打开“适用于Linux的Windows子系统”  
  在控制面板中打开“程序和功能”窗口  
  ![20191226092542.png](https://i.loli.net/2019/12/26/rWEgcmdLkn9qwGV.png)

  点击左侧“启用或关闭Windows功能”，在弹出的窗口中勾选“适用于Linux的Windows子系统”  
  ![20191226092827.png](https://i.loli.net/2019/12/26/z84oHGvNfn5kaEp.png)

- 安装Linux系统  
  在应用商店里选择一个应用(Ubuntu使用频率最高)  
  ![LinuxApp.png](https://i.loli.net/2019/12/26/AwIUSMn1rupX4Y7.png)  
  ![ubuntu.png](https://i.loli.net/2019/12/26/RBGtfPWjHTnvimJ.png)

- 使用说明  

  **安装好了之后，有三种方式可以打开这个 Linux 子系统**  
  a. CMD 命令行窗口运行，使用命令`bash`  
  b. PowerShell 命令行窗口运行，使用命令`bash`  
  c. 使用开始菜单中的快捷方式  

  打开后的 Linux 长这样子，用法与原生 Linux 基本一致(窗体的颜色和透明度可以自己配置)  
  ![20191226093911.png](https://i.loli.net/2019/12/26/NsnMU7hytvPcXYL.png)

  **在 Linux 中访问 Windows 目录**  
  直接访问 /mnt/c， /mnt/d 就可以了，其中c 和 d 代表 Windows 的 C 盘 与 D 盘，无需进行额外的挂载操作。

  **卸载 Linux 子系统**  
  可以用 lxrun 命令(cmd中)  
  lxrun 是 Windows 上 Linux 子系统的管理工具，用法如下：  
  ![ubuntu2.png](https://i.loli.net/2019/12/26/r2CX6W37Uj1ERly.png)

  >近期又测试了一遍，发现lxrun命令在新版本中已被废弃，目前好像还没有替代的命令，对于Ubuntu的话可以用`ubuntu /?`命令，如下图：  
  ![20191226095428.png](https://i.loli.net/2019/12/26/pZeF8Ic9GhuQHsx.png)  
  >参考文章：<https://blog.csdn.net/ijiabao520/article/details/79285041>

>*以上，只是基本的安装步骤和简单体验，有时间会分享更多的玩法*
