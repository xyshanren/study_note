<!--
 * @Autor: 逍遥山人
 * @Date: 2019-12-24 09:18:04
 * @LastEditors: 李逍遥
 * @LastEditTime: 2021-02-13 10:21:56
 * @Descriptiong: 
 -->

# 在Linux上安装Oracle服务的操作步骤 #

- [在Linux上安装Oracle服务的操作步骤](#在linux上安装oracle服务的操作步骤)
  - [闲话少叙，直接开始](#闲话少叙直接开始)
  - [首先，由于服务器比较差，需要先设置swap](#首先由于服务器比较差需要先设置swap)
  - [安装依赖包](#安装依赖包)
  - [创建oracle用户](#创建oracle用户)
  - [修改内核参数](#修改内核参数)
  - [修改用户限制](#修改用户限制)
  - [修改用户验证选项](#修改用户验证选项)
  - [修改用户配置文件](#修改用户配置文件)
  - [安装目录配置](#安装目录配置)
  - [修改用户bash shell](#修改用户bash-shell)
  - [安装图形界面(静默安装则不需要，可参考静默安装文档)](#安装图形界面静默安装则不需要可参考静默安装文档)
  - [安装](#安装)
  - [安装成功后，还需要一些配置](#安装成功后还需要一些配置)

>如题，将我在云服务器上安装Oracle服务的惨痛经历分享出来，期间查找的资料踩过的坑无数，希望对大家能有帮助

## 闲话少叙，直接开始 ##

## 首先，由于服务器比较差，需要先设置swap ##

- 查看是否设置swap虚拟内存  
  交换区（SWAP）要求：按照操作系统推荐配置，根据内存大小，为物理内存的1-1.5倍。  
  推荐：创建2个大小相同、且分布在不同盘（pv）上的SWAP空间。

    ```linux
    free -m
    ```

- 如未设置swap虚拟内存，则设置

    ```linux
    dd if=/dev/zero of=swapfile bs=40960 count=204800
    ```

    >说明：  
    >-if //输入  
    >-of //输出  
    >-bs //块儿大小，单位是byte，最小为40K，该语句表示8000M的大小  
    >-count //总块数

  - 创建Linux交换文件

    ```linux
    mkswap swapfile
    ```

  - 激活swapfile交换文件

    ```linux
    swapon swapfile
    ```

  - 查看是否生效

    ```linux
    free -m
    ```

## 安装依赖包 ##

```linux
yum install binutils compat-libstdc++-33 elfutils elfutils-libelf-devel gcc gcc-c++ glibc glibc-common glibc-devel glibc-headers libaio libaio-devel libgcc libstdc++ libstdc++-devel make sysstat unixODBC unixODBC-devel
```

如下图：  
![clipboard.png](https://i.loli.net/2019/12/24/EOh4JtSk28lanCo.png)

检查下lib是否安装齐全：  

```linux
rpm -q --queryformat %-{name}-%{version}-%{release}-%{arch}"\n" \ compat-libstdc++-33 glibc-kernheaders glibc-headers libaio libgcc glibc-devel
```

## 创建oracle用户 ##

创建Oracle安装组oinstall  

```linux
groupadd oinstall
```

创建数据库管理员组dba  

```linux
groupadd dba
```

创建oracle用户，并分配主组oinstall，其它组：dba  

```linux
useradd -g oinstall -G dba oracle
```

设置密码  

```linux
passwd oracle
```

查看用户属于哪些组  

```linux
groups oracle
```

>如果先创建了oracle用户，则可以用`usermod -g oinstall -G dba oracle`来改变用户的组；

## 修改内核参数 ##

先备份文件  

```linux
cp /etc/sysctl.conf /etc/sysctl.conf.bak
```

修改文件  

```linux
vim /etc/sysctl.conf
```

添加如下参数：  

```linux
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 536870912
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
```

>参数意义参考文档：<http://www.cnblogs.com/colben/p/4120439.html>  
>`kernel.shmall`和`kernel.shmmax`两个值在此次的安装中文件里已有定义，暂时不做修改！

为使上述配置生效而不重启系统，执行如下命令  

```linux
sysctl -p
```

## 修改用户限制 ##

老规矩，先备份文件  

```linux
cp /etc/security/limits.conf /etc/security/limits.conf.bak
```

修改文件  

```linux
vim /etc/security/limits.conf
```

添加如下参数  

```linux
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
```

## 修改用户验证选项 ##

老规矩 ，先备份文件  

```linux
cp /etc/pam.d/login /etc/pam.d/login.bak
```

修改文件  

```linux
vim /etc/pam.d/login
```

添加如下参数  

```linux
session    required     pam_limits.so
```

## 修改用户配置文件 ##

先备份文件  

```linux
cp /etc/profile /etc/profile.bak
```

修改文件  

```linux
vim /etc/profile
```

添加如下参数  

```linux
if [ $USER = "oracle" ]; then
        if [ $SHELL = "/bin/ksh" ]; then
              ulimit -p 16384
              ulimit -n 65536
        else
              ulimit -u 16384 -n 65536
        fi
fi
```

## 安装目录配置 ##

```linux
mkdir -p /u01/oraInventory --创建一个目录树
mkdir /u01/oracle
chown -R oracle:oinstall /u01/ --改变目录的归属人
chmod -R 775 /u01/ --改变目录权限
```

>目录根据实际情况定

## 修改用户bash shell ##

修改文件  

```linux
vim ~/.bash_profile (oracle用户下执行？)
vim /etc/profile (不太了解的推荐用该命令)
```

增加如下参数:  

```linux
export ORACLE_BASE=/u01
export ORACLE_HOME=$ORACLE_BASE/oracle
export ORACLE_SID=smjredw
export PATH=$ORACLE_HOME/bin:$PATH:$HOME/bin
```

如是选择vim ~/.bash_profile命令，则还需执行以下命令  

```linux
su – oracle
env | grep ORA --查看环境变量是否完成
```

>以上命令的区别请参考：<http://www.cnblogs.com/jiaxiaoai/archive/2011/05/22/2053738.html>

## 安装图形界面(静默安装则不需要，可参考静默安装文档) ##

>这里走了一点弯路，分隔线之间的步骤过于复杂，可以跳过，直接看紧随其后的另一种简单的方法
****
查看当前运行级别  

```linux
runlevel
N 3(当前运行级别是3)
```

查看是否安装桌面环境组件  

```linux
yum grouplist | more
```

选择软件组与输入法还有字体等  

```linux
yum groupinstall -y   "Desktop"   "Desktop Platform"   "Desktop Platform Development"　 "Fonts" 　"General Purpose Desktop"　 "Graphical Administration Tools"　 "Graphics Creation Tools" 　"Input Methods" 　"X Window System" 　"Chinese Support [zh]"　"Internet Browser"
```

（临时生效）安装成功后输入startx，或者 init 5, 即可切换到图形界面  
（永久生效）要下次自动启动GNOME进入图形界面操作：
修改`/etc/inittab`文件中的

```linux
id:3:initdefault
# 将3改为5
id:5:initdefault
```

保存后重新启动系统
****

**以上的方式过于麻烦，按以下步骤亦可**  
安装xwindow：  

```linux
yum groupinstall “X Window System”
```

安装gnome：  

```linux
yum groupinstall “Desktop”
```

确认xwindow 能正常显示  
使所有用户都能访问Xserver  
`xhost +`

进入oracle用户：  
切换到图形界面  

```linux
startx

export DISPLAY=:0.0
xclock
```

>因需要使用图形界面，此处需用XShell + Xmanager的方式；  
>参考文章：
<http://blog.csdn.net/wangzhaopeng0316/article/details/14221677>和
<http://blog.csdn.net/linghao00/article/details/8768435>

## 安装 #

- 解压安装包  

    ```linux
    unzip linux.x64_11gR2_database_1of2.zip
    unzip linux.x64_11gR2_database_2of2.zip
    ```

    >会在本目录下面出现一个database的目录  
    >如出现Command not found错误时，需安装压缩解压程序，使用命令：`yum install -y unzip zip`

- 安装  
  在xshell中需利用gnome启动安装程序，这里用vnc远程连接linux系统  

    ```linux
    ./runInstaller
    ```

- 建库  
  需先配置Listener  

    ```linux
    netca
    ```

  弹出配置界面，按步骤配置  
  >参考：<http://blog.chinaunix.net/uid-28329865-id-3453085.html>

- 如报错，则需修改以下两个地方  

    ```linux
    cat /etc/sysconfig/network
    ```

  修改  

    ```linux
    NETWORKING=yes
    HOSTNAME=test11g
    GATEWAY=192.168.1.254
    ```

    ```linux
    cat /etc/hosts
    ```

  修改  

    ```linux
    127.0.0.1               localhost.localdomain localhost test11g
    hostname test11g
    ```

- 如还失败则做以下操作  

  关闭防火墙  

    ```linux
    chkconfig iptables off
    service iptables stop
    vi /etc/selinux/config
    SELINUX=enforcing                          #将此处修改为SELINUX=disabled
    ```

  如此，oracle用户退出重新登录就可以正常启动netca  
  查看是否启动  

    ```linux
    ps –ef | grep LISTENER
    ```

  创建数据库

    ```linux
    dbca
    ```

- 验证  
  使用sql-plus连接  
  命令行下执行`sqlplus /nolog`  
  进入sqlplus提示符，输入  

    ```linux
    connect <username>/<password>@<连接标识符>

    #例子：
    connect report/Sma#007@10.10.xx.224:1521/dw
    #或者
    sqlplus 用户名/密码@192.168.208.xx:1521/orcl
    ```

    >`conn /as sysdba`(本机上可以这样连系统用户)

## 安装成功后，还需要一些配置 ##

- 配置sqlplus：

    ```linux
    show pagesize;
    set pagesize 50000;

    show linesize; 
    set linesize 32767;
    ```

- 创建用户并授权  
  创建用户和密码

    ```linux
    CREATE USER user_name IDENTIFIED BY pwd [DEFAULT TABLESPACE tablespace_name];
    ```

  赋予连接权限

    ```linux
    GRANT CONNECT TO inf_layer;
    ```

    >可先给用户分配到resource角色，再根据需要赋予权限

- 新建表空间  
  例子：

    ```linux
    CREATE TABLESPACE DB_DATA
    LOGGING
    DATAFILE 'D:appAdministratororadataNewDB\DB_DATA.DBF'
    SIZE 32M
    AUTOEXTEND ON
    NEXT 32M MAXSIZE UNLIMITED
    EXTENT MANAGEMENT LOCAL;
    ```

    >'DB_DATA'是你自定义的数据表空间名称，可以任意取名；  
    >'D:appAdministratororadataNewDB\DB_DATA.DBF'是数据文件的存放位置；  
    >'DB_DATA.DBF'文件名也是任意取；  
    >'size 32M'是指定该数据文件的大小，也就是表空间的大小。

**最后感谢以下两篇全程提供指导的文章**  
<http://www.linuxidc.com/Linux/2015-02/113222p4.htm>  
<http://www.cnblogs.com/gaojun/archive/2012/11/22/2783257.html>
