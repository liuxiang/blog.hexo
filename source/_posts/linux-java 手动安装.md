title: linux-java 手动安装
date: 2016-07-19 00:00:04
tags: [ linux , java ]


---


# 安装过程
- 下载安装文件
`官网下载`

http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u80-oth-JPR

       `jdk-7u80-linux-x64.tar.gz`


- 同步至服务器
    `/usr/local/src`



- 安装
     `tar -xvzf  jdk-7u80-linux-x64.tar.gz `


- 环境变量配置
编辑 文件  `vi ~/.bashrc`  添加以下变量
```
export JAVA_HOME=/home/codebrother/jdk/jdk1.7.0_25
export JAVA_BIN=$JAVA_HOME/bin
export JAVA_LIB=$JAVA_HOME/lib
export CLASSPATH=.:$JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar
export PATH=$JAVA_BIN:$PATH
```


- 配置环境变量,方法二
```
在/etc/profile下增加
 
# set Java environment
JAVA_HOME=/usr/share/jdk1.6.0_43
PATH=$JAVA_HOME/bin:$PATH
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME
export PATH
export CLASSPATH
```


---
`Linux JDK安装及配置 (tar.gz版)_百度经验`
http://jingyan.baidu.com/article/ab0b56308966acc15afa7d18.html


`linux如何安装jdk_百度经验`
http://jingyan.baidu.com/article/d621e8dae805272865913fa7.html


<!-- more -->


