# 将Docker Desktop安装到其他盘符的完整指南

>没有梯子的话不建议使用docker。

## 一、下载安装Docker Desktop

### 步骤1：下载Docker Desktop安装程序

手动从 https://www.docker.com/products/docker-desktop/ 下载安装程序。

### 步骤2：手动运行安装程序并指定路径

```powershell
# 以管理员身份运行PowerShell

# 方法A：使用PowerShell命令安装到D盘
Start-Process -Wait -FilePath "D:\Downloads\docker-desktop\docker-desktopInstall.exe" `
  -ArgumentList "install --installation-dir=D:\Programs\Docker --quiet"

# 方法B：使用CMD命令安装到D盘
# start /w "" "D:\Downloads\docker-desktop\docker-desktopInstall.exe" install --installation-dir="D:\Programs\Docker"
```

## 二、迁移Docker数据存储路径（重要！）

即使程序安装到D盘，Docker的镜像和容器数据默认仍存储在C盘。需要额外迁移：

### 步骤1：停止Docker服务

```powershell
# 停止Docker Desktop
Stop-Process -Name "Docker Desktop" -Force -ErrorAction SilentlyContinue

# 停止WSL2
wsl --shutdown
```

### 步骤2：导出WSL数据

```powershell
# 创建备份目录
$backupDir = "D:\DockerBackup"
New-Item -ItemType Directory -Force -Path $backupDir | Out-Null

# 导出docker-desktop-data
wsl --export docker-desktop-data "$backupDir\docker-desktop-data.tar"

# 导出docker-desktop（可选）
wsl --export docker-desktop "$backupDir\docker-desktop.tar"
```

### 步骤3：注销原WSL分发

```powershell
wsl --unregister docker-desktop-data
wsl --unregister docker-desktop
```

### 步骤4：导入到新位置

```powershell
# 创建新的数据存储目录
$newDataPath = "D:\DockerData\WSL"
New-Item -ItemType Directory -Force -Path $newDataPath | Out-Null

# 导入数据到新位置
wsl --import docker-desktop-data "$newDataPath\data" "$backupDir\docker-desktop-data.tar" --version 2
wsl --import docker-desktop "$newDataPath\desktop" "$backupDir\docker-desktop.tar" --version 2
```

### 步骤5：在Docker Desktop中配置

1. 启动Docker Desktop
    
2. 进入 Settings → Resources → Advanced
    
3. 修改 "Disk image location" 为 `D:\DockerData\WSL`
    
4. 点击 "Apply & Restart"
    

## 三、验证安装结果

```powershell
# 验证Docker安装位置
Get-ChildItem "D:\Programs\Docker" -Recurse -Filter "*.exe" | Select-Object Name, Directory

# 验证Docker版本
docker --version
docker-compose --version

# 验证WSL数据位置
wsl -l -v
```

## 四、注意事项和常见问题

### 1. **权限问题**

- 所有操作都需要**管理员权限**
    
- 目标目录需要有写入权限
    
- 建议在D盘创建`Programs`文件夹并设置适当权限
    

### 2. **路径格式要求**

- 使用英文路径，避免中文和特殊字符
    
- 路径不要有空格（或使用引号包裹）
    
- 示例：`D:\Programs\Docker`优于 `D:\我的程序\Docker Desktop`
    

### 3. **软件兼容性**

- 不是所有软件都支持自定义安装路径
    
- Docker Desktop支持`--installation-dir`参数是特例
    
- 其他软件可能需要不同的安装参数
    

### 4. **卸载和清理**

```powershell
# 卸载Docker Desktop（从指定路径）
Start-Process -Wait -FilePath "D:\Programs\Docker\uninstall.exe" -ArgumentList "--quiet"
```

### 5. **回滚到默认设置**

```powershell
# 恢复默认程序安装路径
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion" `
  -Name "ProgramFilesDir" -Value "C:\Program Files"
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion" `
  -Name "ProgramFilesDir (x86)" -Value "C:\Program Files (x86)"
```

## 五、推荐方案总结

### 最推荐的实践流程：

1. **首次安装**：使用上面的完整脚本，一键安装到D盘
    
2. **数据迁移**：按照第三节迁移WSL数据到D盘
    
3. **定期维护**：使用Docker Desktop设置界面管理存储位置
    

### 简化版快速命令：

```powershell
# 保存为 install-docker-d.ps1 并运行
$DockerPath = "D:\Docker"
$Installer = "docker-desktop-installer.exe"

# 下载安装程序
Invoke-WebRequest -Uri "https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe" -OutFile "$env:TEMP\$Installer"

# 安装到D盘
Start-Process -Wait -FilePath "$env:TEMP\$Installer" -ArgumentList "install --installation-dir=`"$DockerPath`" --quiet"

# 启动Docker
Start-Process "$DockerPath\Docker\Docker Desktop.exe"
```

通过以上步骤，你可以成功将Docker Desktop安装到其他盘符，并有效节省C盘空间。如果在操作过程中遇到问题，建议先检查路径权限和磁盘空间，然后查看Docker Desktop的安装日志。

