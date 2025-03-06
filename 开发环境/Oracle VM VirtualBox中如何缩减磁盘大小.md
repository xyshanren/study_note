---
title: Oracle VM VirtualBox中如何缩减磁盘大小
author:
  - 守一
created: 2025-03-05
description: 在Oracle VM VirtualBox中，分配给虚拟机的磁盘太大需要缩减大小的时候可以参考本文档
tags:
  - 虚拟机环境
  - VirtualBox
---
## VirtualBox命令行工具收缩磁盘

- 打开命令提示符或者终端。
- 导航到VirtualBox的安装目录。
- 执行以下命令来收缩虚拟磁盘：
```shell
VBoxManage modifyhd mydisk.vdi --compact
```

**注意事项
- 在执行收缩操作前，确保已备份数据。
- 如果虚拟磁盘是固定大小的，需要先将其转换为动态磁盘才能进行收缩操作。
- 虚拟磁盘固定大小与动态分配方式之间的转换请参考 [VirtualBox虚拟硬盘固定大小与动态分配方式之间的转换](https://leeguangxing.cn/blog_post_81.html)

## VBoxManage 的详细教程

以下是关于VirtualBox命令行工具`VBoxManage`的详细教程，涵盖核心功能及常用操作：

---

### 基础命令

#### 列出虚拟机与配置信息  
- 列出所有虚拟机：  
```bash
VBoxManage list vms
```
- 列出正在运行的虚拟机：  
```bash
VBoxManage list runningvms
```
- 查看虚拟机详细信息：  
```bash
VBoxManage showvminfo "虚拟机名称"
```
- 查看当前所有的虚拟硬盘
```bash
vboxmanage list hdds
```

#### 支持的虚拟化类型
- 查看所有支持的操作系统类型：  
```bash
VBoxManage list ostypes
```
- 查看虚拟磁盘类型支持：  
```bash
VBoxManage list hddbackends
```

---

### 虚拟机生命周期管理

1. 创建虚拟机  
```bash
VBoxManage createvm --name "VM_Name" --ostype "操作系统类型" --register
``` 
   - 示例：创建名为`Ubuntu22`的虚拟机：  
```bash
VBoxManage createvm --name "Ubuntu22" --ostype Ubuntu_64 --register
```
 
2. 启动与关闭虚拟机  
   - 启动虚拟机（图形界面）：  
```bash
VBoxManage startvm "虚拟机名称" --type gui
```
   - 无界面启动（适用于服务器）：  
```bash
VBoxManage startvm "虚拟机名称" --type headless
```
   - 强制关闭虚拟机：  
```bash
VBoxManage controlvm "虚拟机名称" poweroff
```
 

3. 暂停与恢复虚拟机  
```bash
VBoxManage controlvm "虚拟机名称" pause   # 暂停
VBoxManage controlvm "虚拟机名称" resume  # 恢复
``` 

### 磁盘与存储管理

1. 创建虚拟硬盘  
   - 动态分配磁盘：  
```bash
VBoxManage createmedium disk --filename "disk.vdi" --size 20480 --variant Standard
```
   - 固定大小磁盘：  
```bash
VBoxManage createmedium disk --filename "disk.vdi" --size 20480 --variant Fixed
```

2. 挂载磁盘与光驱
   - 添加IDE控制器并挂载硬盘：  
```bash
VBoxManage storagectl "虚拟机名称" --name "IDE Controller" --add ide --controller PIIX4
VBoxManage storageattach "虚拟机名称" --storagectl "IDE Controller" --port 0 --device 0 --type hdd --medium "disk.vdi"
```
   - 挂载ISO镜像：  
```bash
VBoxManage storageattach "虚拟机名称" --storagectl "IDE Controller" --port 1 --device 0 --type dvddrive --medium "ubuntu.iso"
```  

### 网络配置

1. 设置网络模式  
   - 桥接模式：  
```bash
VBoxManage modifyvm "虚拟机名称" --nic1 bridged --bridgeadapter1 eth0
``` 
   - NAT模式：  
```bash
VBoxManage modifyvm "虚拟机名称" --nic1 nat
```

2. 端口转发  
```bash
VBoxManage modifyvm "虚拟机名称" --natpf1 "规则名,tcp,,宿主机端口,,虚拟机端口"
```
   - 示例：将宿主机的8080转发到虚拟机的80端口：  
```bash
VBoxManage modifyvm "虚拟机名称" --natpf1 "web,tcp,,8080,,80"
``` 

### 高级功能

1. 快照管理  
   - 创建快照：  
```bash
VBoxManage snapshot "虚拟机名称" take "快照名称"
```
   - 恢复快照：  
```bash
VBoxManage snapshot "虚拟机名称" restore "快照名称"
```

2. 克隆虚拟机
```bash
VBoxManage clonevm "原虚拟机名称" --name "新虚拟机名称" --register
```

3. 克隆磁盘
```bash
# 语法
VBoxManage clonemedium <uuid | source-medium> <uuid | target-medium> [disk | dvd | floppy] [--existing] [--format=VDI | VMDK | VHD | RAW | other ] [--variant=Standard,Fixed,Split2G,Stream,ESX]

# 示例1，从固定大小转换为动态分配
vboxmanage clonemedium "source.vdi" "target.vdi" disk -variant Standard

# 示例2，从动态分配转换为固定大小
vboxmanage clonemedium "source.vdi" "target.vdi" disk -variant Fixed
```

4. 远程桌面控制
   - 启用VRDE服务：  
```bash
VBoxManage modifyvm "虚拟机名称" --vrde on
``` 
   - 设置远程桌面端口：  
```bash
VBoxManage modifyvm "虚拟机名称" --vrdeport 3389
``` 

### 注意事项
1. 路径问题：操作时需确保路径正确，尤其是虚拟磁盘和ISO文件的绝对路径。
2. 权限要求：部分命令需管理员权限（如Linux下的`sudo`）。
3. 扩展包支持：高级功能（如USB 2.0/3.0）需安装`VirtualBox Extension Pack`。

如需更完整的命令列表，可参考VirtualBox官方文档或通过`VBoxManage -h`查看帮助。