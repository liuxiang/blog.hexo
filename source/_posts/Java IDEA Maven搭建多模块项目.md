title:  Java IDEA Maven搭建多模块项目


date: 2017-5-13 00:00:00
tags: [ Maven ]



---


# 主工程
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/95754311.jpg)


![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/54843808.jpg)

 
# 子模块（web/或不选）
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/87323924.jpg)


![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/42850945.jpg)


# 预览
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/1742924.jpg)


![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/17956154.jpg)



---
# Maven 多模块项目参考 iBase4J

http://git.oschina.net/iBase4J/iBase4J


` #32 iBase4J 架构研究 [置顶]` http://git.oschina.net/iBase4J/iBase4J/issues/32
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/85002761.jpg)


# 快速构建
```
* 启动命令：
     clean package -P build tomcat7:run-war-only -f pom-sys-service-server.xml
     clean package -P build tomcat7:run-war-only -f pom-sys-web-server.xml
* 打包命令：
     clean package -P build -f pom-sys-service-server.xml
     clean package -P build -f pom-sys-service-server.xml
* 生产环境打包命令：
     clean package -P product -f pom-sys-service-server.xml
     clean package -P product -f pom-sys-service-server.xml
```


-  pom-sys-service-server.xml (方便在主项目iBase4j中指定具体模块的构建)
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<artifactId>iBase4J-SYS-Service-Server</artifactId>
<name>${project.artifactId}</name>
<packaging>pom</packaging>
<url>http://maven.apache.org</url>
<parent>
<groupId>org.ibase4j</groupId>
<artifactId>iBase4J</artifactId>
<version>1.1.0</version>
<relativePath>./</relativePath>
</parent>
<modules>
<module>iBase4J-Common</module>
<module>iBase4J-SYS-Facade</module>
<module>iBase4J-SYS-Service</module>
</modules>
<!-- clean package -P build tomcat7:run-war-only -f pom-sys-service-server.xml -->
</project>
```


---
# 小技巧
功能搜索`Ctrl+shift+A`-`reim` 选择`Reimport`即可`刷新 工程 maven`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-21/41633512.jpg)


---
**参考**
`Java一步一步构建web系统 在IDEA下用Maven搭建多模块项目 - 推酷`  `★`
http://www.tuicool.com/articles/FZfiYfF


`[工具资源] Intellij IDEA 2016 学习系列之 (一) 创建 maven 多模块项目 - 推酷`  `★`
http://www.tuicool.com/articles/z6ZZzuR


`每日一博 | Maven 多模块项目介绍及搭建 - 推酷` `★`
http://www.tuicool.com/articles/yiIra2


`Maven提高篇系列之（一）——多模块 vs 继承`

www.tuicool.com/articles/RJBZb2z


`使用命令行快速创建Maven多模块项目 - 推酷`
http://www.tuicool.com/articles/Y3Ab2uJ


---
`用idea创建一个maven web项目 - Coder Wu的博客 - 博客频道 - CSDN.NET`
http://blog.csdn.net/sinat_34596644/article/details/52891274


`使用IntelliJ IDEA 15和Maven创建Java Web项目 - MyArrow的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/myarrow/article/details/50824793


