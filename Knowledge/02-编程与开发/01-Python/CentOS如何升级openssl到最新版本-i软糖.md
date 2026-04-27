---
url: https://www.4spaces.org/how-to-upgrade-openssl-on-centos-7/
source: www.4spaces.org
date_saved: 2026-04-23
tags: Python, DevOps, 设计
content_type: article
word_count: 2475
status: 已处理
---

# CentOS如何升级openssl到最新版本-i软糖

> source: [www.4spaces.org](https://www.4spaces.org/how-to-upgrade-openssl-on-centos-7/)

## 核心要点

1. 当前位置：i软糖  博文  正文
2. sudo yum -y install perl perl-devel gcc gcc-c++
3. [michael@centos7 src]$ openssl version
4. [michael@centos7 ~]$ cd /usr/local/src
5. [michael@centos7 src]$ wget https://github.
## 内容预览

当前位置：i软糖  博文  正文

环境信息

CentOS Linux release 7.6.1810 (Core)；

OpenSSL 1.0.2k-fips 26 Jan 2017；

OpenSSL 1.1.1c 28 May 2019

依赖

sudo yum -y install perl perl-devel gcc gcc-c++

升级

查看当前版本

[michael@centos7 src]$ openssl version
OpenSSL 1.0.2k-fips 26 Jan 2017

下载最新版本

当前最新版本是OpenSSL_1_1_1c（2019年7月5日），请到下面页面下载。

官网下载地址： https://www.openssl.org/source/

Github地址：https://github.com/openssl/openssl/releases

这里下载到/usr/local/src目录，

[michael@centos7 ~]$ cd /usr/local/src

[michael@centos7 src]$ wget https://github.com/openssl/openssl/archive/OpenSSL_1_1_1c.tar.gz

[michael@centos7 src]$ tar xzvf ./OpenSSL_1_1_1c.tar.gz

[michael@centos7 src]$ cd openssl-OpenSSL_1_1_1c/

接下来执行编译操作，

[michael@centos7 src]$ ./config

如果没有安装Perl 5，执行config会有提示没有安装，需要先进行安装，执行sudo yum install perl。

接下来依次执行下面的命令：

[michael@centos7 src]$ make
[michael@centos7 src]$ make test
[michael@centos7 src]$ sudo make install

替换新旧版本：

[michael@centos7 src]$ sudo mv /usr/bin/openssl /usr/bin/oldopenssl

[michael@centos7 src]$ sudo ln -s /usr/local/bin/openssl /usr/bin/openssl

如果执行openssl version报下面错误，

[inspur@localhost openssl-OpenSSL_1_1_1c]$ openssl version
openssl: error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory

则执行下面命令解决：

[michael@centos7 src]$ sudo ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/
[michael@centos7 src]$ sudo ln -s /usr/local/lib64/libcrypto.so.1.1 /usr/lib64/

然后查看当前版本：

michael@cent...
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
