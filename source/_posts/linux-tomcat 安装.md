title: linux-tomcat 安装
date: 2016-07-19 00:00:02
tags: [linux,tomcat]


---

# tomcat依赖
- java (jdk-7u80)
请见： ` linux-java yum安装.md `  ` linux-java 手动安装.md `
- 检查 `java -version`


# tomcat安装


- 下载
```
cd /usr/local/src
wget http://mirror.bit.edu.cn/apache/tomcat/tomcat-7/v7.0.70/bin/apache-tomcat-7.0.70.tar.gz
```


- 安装
```
tar -zxvf  apache-tomcat-7.0.70.tar.gz
mv  apache-tomcat-7.0.70   /usr/local/
```


- 部署
```
# 服务部署
souceCode 同步至 /usr/local/apache-tomcat-7.0.70/webapps

# jvm配置
vi catalina.sh
```


- 多个tomcat,需更新环境变量
```
vi /usr/local/apache-tomcat-7.0.70/bin/startup.sh



CATALINA_BASE="/usr/local/apache-tomcat-7.0.62"
CATALINA_HOME="/usr/local/apache-tomcat-7.0.62"
```
> windows
set "CATALINA_BASE=/usr/local/apache-tomcat-7.0.62"
set  " CATALINA_HOME=/usr/local/apache-tomcat-7.0.62"


- 启动
```

# 启动tomcat
/usr/local/apache-tomcat-7.0.70/bin/startup.sh

# 同步查看启动日志
tail -f  /usr/local/apache-tomcat-7.0.70/logs/ catalina.out
```


- 数据库白名单（如果需要）


- 访问测试
```
http://ip:8080/appName
```


- 停止
```
/usr/local/apache-tomcat-7.0.70/bin/startup.sh

```


---


`CentOS7.0安装与配置Tomcat-7`
http://www.centoscn.com/image-text/config/2014/1127/4194.html


---


`linux安装tomcat_百度经验`
http://jingyan.baidu.com/article/ff42efa9162ea5c19e22021c.html


`Tomcat在Linux上的安装与配置 - 平凡的世界 - 博客频道 - CSDN.NET`
http://blog.csdn.net/gyming/article/details/36060843


`Linux下安装Tomcat服务器和部署Web应用 - 孤傲苍狼 - 博客园`
http://www.cnblogs.com/xdp-gacl/p/4097608.html


<!-- more -->
