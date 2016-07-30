title: linux-java yum安装
date: 2016-07-19 00:00:03
tags: [linux,yum,java]


---


# 查看CentOS自带JDK是否已安装
```
yum list installed |grep java
```
- 若有自带安装的JDK，如何卸载CentOS系统自带Java环境？
 - 卸载JDK相关文件输入：yum -y remove java-1.7.0-openjdk*。
 - 卸载tzdata-java输入：yum -y remove tzdata-java.noarch。
 
--------------------------------------------------------------------
# 查看yum库中的Java安装包
```
yum -y list java*
```


# 使用yum安装Java环境。
```
yum -y install java-1.7.0-openjdk*
(以yum库中java-1.7.0为例)
```
 
# 查看刚安装的Java版本信息。
```
java -version 可查看Java版本
javac 可查看Java的编译器命令用法（可略）。
默认情况下jdk安装得路径 /usr/lib/jvm

```


**参考**
`在CentOS上安装Java环境：[1]使用yum安装java_百度经验`
http://jingyan.baidu.com/article/4853e1e51d0c101909f72607.html
 
`在linux上使用yum安装JDK-qyf404-ChinaUnix博客`
http://blog.chinaunix.net/uid-15463753-id-4252690.html


<!-- more -->
