title: hybrid——phonegap+ionic 环境安装
date: 2015-10-20 00:00:00 #发表日期，一般不改动
categories: hybrid #文章文类
tags: [hybrid,phonegap,ionic] #文章标签，多于一项时用这种格式
photos:

---
# 基础环境
`nodejs`    https://nodejs.org/en/download/
`Ant`    http://mirror.bit.edu.cn/apache//ant/binaries/apache-ant-1.9.6-bin.zip
`java`        
* `new jdk8` http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
* `archive` http://www.oracle.com/technetwork/java/javase/archive-139210.html

---
`环境搭建`  http://blog.csdn.net/aaawqqq/article/details/19755179/
`学习网站`  http://www.phonegap100.com | 第一讲 phonegap 性能优化 以及 phonegap + Angularjs +ionic.pdf

# 系统环境变量
```vala
# JAVA_HOME
C:\Program Files\Java\jdk1.8.0_66
   
# ANT_HOME
G:\workTool\apache-ant-1.9.4

# CLASSPATH
%JAVA_HOME%\libr;%ANT_HOME%\lib

# path
## 检查nodejs
C:\Program Files\nodejs\

## java环境
%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin
%ANT_HOME%\bin

## android sdk
G:\IDE\adt-bundle\sdk\platform-tools
G:\IDE\adt-bundle\sdk\tools

## 未知是否必填
G:\IDE\adt-bundle\eclipse\plugins\org.apache.ant_1.8.3.v201301120609\bin
```
参考: http://blog.csdn.net/aaawqqq/article/details/19755179/

<!-- more -->

# 安装phonegap
windows>`npm install -g phonegap`
Mac使用>`sudo npm install -g phonegap`
 
# 等待安装   完成后安装 cordova:
windows>`npm install -g cordova`                               
Mac使用>`sudo npm install -g cordova`

## 安装完成后,检验:
`phonegap -version`
`cordova -version`

### 创建phonegap工程(如使用`ionic`请跳过)
`phonegap create my-app`
`cd my-app`
`phonegap run android`

# 安装ionic
windows>`npm install -g ionic`
Mac使用>`sudo npm install -g ionic`

## 创建运行项目
`ionic start myproject`
`cd myproject`

#### Android
* `ionic platform add android`
* `ionic build android`
* `ionic emulate android` # 模拟器运行
* `ionic run android` # 连接上手机运行

### ios
* `ionic platform add ios`
* `ionic build ios`
* `ionic emulate ios`

---
工程结构
>js是控制层
app.js是路由配置页面
controllers.js是各个模块的业务逻辑
templates是视图
 
线上打包[android ,ios]
`http://build.phonegap.com`（ios需要证书）