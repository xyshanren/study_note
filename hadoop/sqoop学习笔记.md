<!--
 * @Autor: 李逍遥
 * @Date: 2021-02-17 09:56:30
 * @LastEditors: 李逍遥
 * @LastEditTime: 2021-02-17 14:49:21
 * @Descriptiong: 
-->

# sqoop的学习之路 #

- [sqoop的学习之路](#sqoop的学习之路)
  - [概述](#概述)
  - [安装](#安装)
    - [前提概述](#前提概述)
    - [下载](#下载)
    - [安装步骤](#安装步骤)
  - [sqoop的使用](#sqoop的使用)
    - [基本操作](#基本操作)
    - [连接MySQL](#连接mysql)

## 概述 ##

Sqoop是一款开源的工具，主要用于在Hadoop(Hive)与传统的数据库(mysql、postgresql…)间进行数据的传递，可以将一个关系型数据库（例如 ： MySQL ,Oracle ,Postgres等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。Sqoop项目开始于2009年，最早是作为Hadoop的一个第三方模块存在，后来为了让使用者能够快速部署，也为了让开发人员能够更快速的迭代开发，Sqoop独立成为一个Apache项目。

**核心功能**有两个：导入数据和导出数据。

**导入数据**：从MySQL，Oracle 导入数据到 Hadoop 的 HDFS、HIVE、HBASE 等数据存储系统。

**导出数据**：从 Hadoop 的文件系统中导出数据到关系数据库（mysql等）。Sqoop 的本质还是一个命令行工具，和 HDFS，Hive 相比，并没有什么高深的理论。

sqoop工具的本质就是迁移数据， 迁移的方式：就是把sqoop的迁移命令转换成MR程序。

## 安装 ##

### 前提概述 ###

sqoop就是一个工具， 只需要在一个节点上进行安装即可。
如果如果需要sqoop工具进行hive、hbase等的系统和MySQL之间的交互，那安装SQOOP的节点一定要包含以上你要使用的集群或者系统。

### 下载 ###

下载地址(使用清华的镜像)：<https://mirrors.tuna.tsinghua.edu.cn/apache/sqoop/>

- **sqoop版本说明**
    sqoop1与sqoop2完全不兼容，绝大部分企业所使用的sqoop的版本都是 sqoop1。
    sqoop1：目前是 sqoop-1.4.6、sqoop-1.4.7
    sqoop2：sqoop-1.99.4及以后的版本（目前到1.99.7）

### 安装步骤 ###

- 上传解压安装包到指定目录
  也可以直接使用 `wget` 命令直接下载到服务器中。

    ```shell
    # 以下命令以用户 hadoop，安装目录为 /usr/local 为例
    # 解压
    tar -zxvf sqoop-1.4.7.bin__hadoop-2.6.0.tar.gz -C /usr/local
    cd /usr/local
    # 修改文件名
    mv sqoop-1.4.7.bin__hadoop-2.6.0 sqoop
    # 修改文件夹属主
    sudo chown -R hadoop:hadoop sqoop
    ```

- 修改配置文件sqoop-env.sh

    ```shell
    cd sqoop/conf/
    # 将sqoop-env-template.sh复制一份并命名为sqoop-env.sh
    cat sqoop-env-template.sh  >> sqoop-env.sh
    # 编辑
    vim sqoop-env.sh
    ```

    修改sqoop-env.sh的如下信息

    ```shell
    # 在脚本中找到以下命令，将注释取消并填上相应的值
    export HADOOP_COMMON_HOME=/usr/local/hadoop
    export HADOOP_MAPRED_HOME=/usr/local/hadoop
    export HIVE_HOME=/usr/local/hive
    ```

- 修改sqoop参数验证配置
  进入bin目录，找到 `configure-sqoop` 文件，注释掉其中未用到的参数检验命令，如下：

    ```shell
    ### Moved to be a runtime check in sqoop.
    #if [ ! -d "${HBASE_HOME}" ]; then
    #  echo "Warning: $HBASE_HOME does not exist! HBase imports will fail."
    #  echo 'Please set $HBASE_HOME to the root of your HBase installation.'
    #fi
    #
    ### Moved to be a runtime check in sqoop.
    #if [ ! -d "${HCAT_HOME}" ]; then
    #  echo "Warning: $HCAT_HOME does not exist! HCatalog jobs will fail."
    #  echo 'Please set $HCAT_HOME to the root of your HCatalog installation.'
    #fi
    #
    #if [ ! -d "${ACCUMULO_HOME}" ]; then
    #  echo "Warning: $ACCUMULO_HOME does not exist! Accumulo imports will fail."
    #  echo 'Please set $ACCUMULO_HOME to the root of your Accumulo installation.'
    #fi
    #if [ ! -d "${ZOOKEEPER_HOME}" ]; then
    #  echo "Warning: $ZOOKEEPER_HOME does not exist! Accumulo imports will fail."
    #  echo 'Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.'
    #fi
    ```

    本次未用到HBASE、zookeeper等。

- 配置环境变量
  使用命令 `vim ~/.bashrc`
  在配置文件中添加如下信息：

    ```txt
    export SQOOP_HOME=/usr/local/sqoop
    export PATH=$PATH:$SQOOP_HOME/bin
    ```

  保存该文件，退出vim编辑器，然后执行 `source ~/.bashrc` 命令，使配置生效。

- 将mysql驱动包拷贝到 `$SQOOP_HOME/lib` 目录
  驱动包：mysql-connector-java-5.1.40-bin.jar 或者最新版的 mysql-connector-java-8.0.20.jar

- 验证安装是否成功
  使用命令 `sqoop-version` 或 `sqoop version`

## sqoop的使用 ##

### 基本操作 ###

- 可以使用 `sqoop help` 命令查看sqoop 支持哪些命令

    ```shell
    [hadoop@localhost ~]$ sqoop help
    21/02/17 14:28:16 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
    usage: sqoop COMMAND [ARGS]

    Available commands:
    codegen            Generate code to interact with database records
    create-hive-table  Import a table definition into Hive
    eval               Evaluate a SQL statement and display the results
    export             Export an HDFS directory to a database table
    help               List available commands
    import             Import a table from a database to HDFS
    import-all-tables  Import tables from a database to HDFS
    import-mainframe   Import datasets from a mainframe server to HDFS
    job                Work with saved jobs
    list-databases     List available databases on a server
    list-tables        List available tables in a database
    merge              Merge results of incremental imports
    metastore          Run a standalone Sqoop metastore
    version            Display version information

    See 'sqoop help COMMAND' for information on a specific command.
    [hadoop@localhost ~]$
    ```

  得到这些支持的命令之后，如果不知道使用方式，可以使用 `sqoop help command` 的方式 来查看某条具体命令的使用方式，比如： `sqoop help import` 。

### 连接MySQL ###

- 列出所有的库名
  使用 `list-databases` 命令

    ```shell
    sqoop list-databases --connect jdbc:mysql://localhost:3306/ --username root -P
    ```

- 列出库中所有的表名

    ```shell
    sqoop list-tables --connect jdbc:mysql://localhost:3306/ --username root -P
    ```

