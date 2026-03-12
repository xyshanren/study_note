## 一、Ubuntu 版本推荐

**强烈推荐 Ubuntu 24.04 LTS（长期支持版）**

**推荐理由：**

- **长期支持**：提供 5 年安全更新和维护，稳定性高
    
- **最新特性**：搭载 Linux 6.8 内核、GNOME 46 桌面环境、Python 3.12、OpenJDK 21
    
- **兼容性好**：与 VirtualBox 7.0+ 版本兼容性最佳
    
- **社区支持**：作为最新 LTS 版本，有丰富的教程和问题解决方案
    

**其他可选版本：**

- Ubuntu 22.04 LTS：成熟稳定，仍有多年支持
    
- Ubuntu 20.04 LTS：即将结束支持，仅适合特定兼容性需求
    

## 二、安装前准备

1. **下载 VirtualBox**：从官网下载最新版（建议 7.0.20+）
    
2. **下载 Ubuntu 24.04 LTS ISO**：从 Ubuntu 官网或国内镜像站（如清华镜像）下载 64 位桌面版
    
3. **验证硬件虚拟化**：确保 BIOS/UEFI 中已启用 Intel VT-x 或 AMD-V 虚拟化技术
    

## 三、创建 Ubuntu 虚拟机

|配置项|推荐值|说明|
|---|---|---|
|名称|Ubuntu 24.04 LTS|自定义名称|
|类型|Linux|固定选项|
|版本|Ubuntu (64-bit)|必须选择 64 位版本|
|内存|4GB（4096MB）|最低 2GB，推荐 4GB 以上|
|虚拟硬盘|VDI 格式，动态分配|兼容性最好|
|硬盘大小|30GB+|建议 30-50GB|

**关键配置调整：**

1. **系统设置**：启用 EFI（推荐），处理器核心数设为 2（主机核心数一半）
    
2. **显示设置**：显存 128MB，**禁用 3D 加速**（避免 Wayland 兼容性问题）
    
3. **存储设置**：在“控制器:IDE”中添加 Ubuntu ISO 文件
    
4. **网络设置**：默认 NAT 模式（虚拟机可访问互联网）
    

## 四、安装 Ubuntu 系统

1. **启动虚拟机**：点击“启动”，从 ISO 引导
    
    - 如遇黑屏：重启虚拟机，在 Grub 菜单按 `E`键，在 `linux`行末尾添加 `nomodeset`，按 F10 继续
        
    
2. **图形化安装步骤：**
    
    - **语言与键盘**：选择中文（简体）
        
    - **更新与软件**：选择“正常安装”，勾选“安装第三方软件”（支持 MP3、显卡驱动等）
        
    - **分区方案**：新手选择“清除整个磁盘并安装 Ubuntu”（自动分区）
        
    - **时区与用户**：选择“上海”，创建用户名和密码
        
    - **安装完成**：点击“现在重启”，按提示移除 ISO 文件
        
    

## 五、安装后优化配置

1. **安装 VirtualBox 增强功能**：
    
    ```shell
    # 更新系统并安装依赖
    sudo apt update && sudo apt upgrade -y
    sudo apt install build-essential dkms linux-headers-$(uname -r) -y
    
    # 安装增强功能
    sudo /media/$USER/VBox_GAs_*/VBoxLinuxAdditions.run
    sudo reboot
    ```
    
    安装后启用：共享剪贴板（设备→共享剪贴板→双向）、拖放文件、自适应分辨率
    
2. **配置共享文件夹**：
    
    - VirtualBox 设置→共享文件夹→添加主机文件夹
        
    - Ubuntu 中执行：`sudo usermod -aG vboxsf $USER`并重启
        
    
3. **更换国内源加速**：
    
    ```shell
    sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
    sudo sed -i 's/archive.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list
    sudo apt update
    ```
    

## 六、常见问题解决

1. **黑屏或分辨率异常**：登录时选择“Ubuntu on Xorg”（禁用 Wayland），或永久禁用 Wayland
    
2. **网络连接失败**：检查 NAT 设置，或更新 VirtualBox 至最新版
    
3. **虚拟化未启用**：进入 BIOS 启用 Intel VT-x/AMD-V
    

## 七、使用建议

1. **创建快照**：安装完成后，在 VirtualBox 菜单栏点击“机器→拍摄快照”，便于回滚
    
2. **资源分配**：根据主机配置调整内存和处理器核心数，避免资源竞争
    
3. **定期更新**：保持 Ubuntu 系统和 VirtualBox 为最新版本
    

按照以上步骤，你可以在 VirtualBox 中顺利安装并运行 Ubuntu 24.04 LTS 虚拟机。整个过程的关键在于正确配置虚拟机参数，特别是禁用 3D 加速以避免显示问题。