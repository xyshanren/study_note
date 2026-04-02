## 一、版本推荐
AnolisOS-8.10-x86_64-minimal 尺寸小

## 二、创建虚拟机和安装系统
参照：[[在 Oracle VM VirtualBox 中安装 Ubuntu 虚拟机]]

## 三、配置静态IP
参照：[[为virtualbox虚拟机设置静态ip]]

## 四、配置开发环境
### 1.先更新dnf
```shell
dnf update

# 如果习惯使用vim
dnf install vim

# 安装wget等常用工具
dnf install wget
```

### 2.安装ssh服务
```shell
dnf install openssh-server -y
# 启动
systemctl start sshd
# 设置开机启动
systemctl enable sshd
# 查看状态
systemctl status sshd
# 如果系统防火墙（firewalld）已启用，可能需要开放 SSH 默认的 22 端口
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload
```

### 3.安装python
```shell
# 安装指定版本的python
dnf install python39  # 安装Python 3.9
dnf install python3.11  # 安装Python 3.11

# 安装对应版本的pip
dnf install python3.11-pip

python3 --version
pip3.11 --version

# 需要升级 pip3.11 自身可以用以下命令
pip3.11 install --upgrade pip
```

### 4.对于需要多python版本的情况建议使用 poetry 管理环境
- 安装
```shell
curl -sSL https://install.python-poetry.org | python3 -
```
安装后需将 `$HOME/.local/bin`加入 PATH（或重启终端）。

- 基本使用
```shell
poetry new myproject      # 创建新项目
cd myproject
poetry add requests       # 添加依赖（自动创建/激活虚拟环境）
poetry install            # 安装所有依赖
poetry shell              # 激活虚拟环境
```

详细教程参照：[[Poetry 安装使用教程]]
### 5.安装和使用docker
参照：[[Linux中安装docker及docker的使用]]

```shell
# 卸载旧版本（如有）
dnf remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine

# 添加 Docker 官方仓库
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 添加 aliyun 仓库
dnf config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 安装依赖工具
dnf install -y dnf-plugins-core

# 上一步如果失败，比如repo错误则重新配置源
# 删除之前添加的错误的仓库配置文件
rm -f /etc/yum.repos.d/docker-ce.rep*
# 配置官方的
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 清理旧缓存
dnf clean all

# 更新元数据缓存
dnf makecache

# 重新执行安装 Docker CE 的命令
dnf install -y dnf-plugins-core
dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 启动 Docker 服务
systemctl start docker

# 设置开机自启
systemctl enable docker

# 验证安装
docker --version

# 添加 docker 用户组
groupadd docker

# 将当前用户加入 docker 组
usermod -aG docker $USER

# 重新登录或执行以下命令使组更改生效
newgrp docker

# 验证无需 sudo 运行
docker run hello-world
```

如果阿里云的镜像有问题的话可以配置清华镜像
```shell
# 移除所有 Docker 相关的仓库文件
rm -f /etc/yum.repos.d/docker*.repo

# 清理 DNF 缓存
dnf clean all

tee /etc/yum.repos.d/docker-ce.repo << 'EOF'
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/gpg

[docker-ce-stable-debuginfo]
name=Docker CE Stable - Debuginfo $basearch
baseurl=https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/$releasever/debug-$basearch/stable
enabled=0
gpgcheck=1
gpgkey=https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/gpg

[docker-ce-stable-source]
name=Docker CE Stable - Sources
baseurl=https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/$releasever/source/stable
enabled=0
gpgcheck=1
gpgkey=https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/gpg
EOF

# 更新 DNF 缓存
dnf makecache

# 安装必要工具
dnf install -y dnf-plugins-core

# 安装 Docker 及相关组件
dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 检查 Docker 版本
docker --version
docker compose version

# 启动 Docker 服务
systemctl start docker
# 设置开机自启
systemctl enable docker

# 运行测试容器
docker run hello-world
```
