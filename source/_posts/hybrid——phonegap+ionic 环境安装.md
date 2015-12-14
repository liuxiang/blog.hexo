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
 
# 等待安装   完成后安装 cordova
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
* `cordova prepare` # 刷新前端代码与platform代码同步

### ios
* `ionic platform add ios`
* `ionic build ios`
* `ionic emulate ios`

### 检查安装
`cordova plugin list`

### adb常规命令
`adb devices` 查询模拟器/设备实例

给特定的模拟器/设备实例发送命令
`adb -s <serialNumber> <command>`  
`adb -s emulator-5556 install helloWorld.apk` 

安装&卸载软件
`adb install <path_to_apk>` 
`adb uninstall <软件名/包名>`

进入模拟器的shell模式
`adb shell`
 
缷载apk包
`adb shell`
`cd data/app`
`rm 123.apk`

服务启停
`adb kill-server`
`adb start-server`

---
工程结构
>js是控制层
app.js是路由配置页面
controllers.js是各个模块的业务逻辑
templates是视图
 
线上打包[android ,ios]
`http://build.phonegap.com`（ios需要证书）

# 问题处理篇: ionic build android 执行空白
如下
```
F:\work\wosai\code.wosai\edu.k12\k12student\k12student>ionic build android

F:\work\wosai\code.wosai\edu.k12\k12student\k12student>
```
解决思路:
在正确的可build的环境中执行,查看执行的内容.
```
F:\work\wosai\code.wosai\edu.k12\k12student\k12student>ionic build android
Running command: "C:\Program Files\nodejs\node.exe" F:\work\wosai\code.wosai\edu.k12\k12student\k12student\hooks\after_prepare\010_add_platform_class.js F:\work\wosai\code.wosai\edu.k12\k12student\k12student
add to body class: platform-android
Running command: cmd "/s /c "F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\cordova\build.bat""
ANDROID_HOME=G:\IDE\adt-bundle\sdk
JAVA_HOME=C:\Program Files\Java\jdk1.8.0_66
Running: F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\gradlew cdvBuildDebug -b F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\build.gradle -Dorg.gradle.daemon=true
:preBuild
:compileDebugNdk UP-TO-DATE
:preDebugBuild
:checkDebugManifest
:CordovaLib:compileLint
:CordovaLib:copyDebugLint UP-TO-DATE
:CordovaLib:mergeDebugProguardFiles UP-TO-DATE
:CordovaLib:preBuild
:CordovaLib:preDebugBuild
:CordovaLib:checkDebugManifest
:CordovaLib:prepareDebugDependencies
:CordovaLib:compileDebugAidl UP-TO-DATE
:CordovaLib:compileDebugRenderscript
```
其中关键命令:
```
 "C:\Program Files\nodejs\node.exe" F:\work\wosai\code.wosai\edu.k12\k12student\k12student\hooks\after_prepare\010_add_platform_class.js F:\work\wosai\code.wosai\edu.k12\k12student\k12student

cmd "/s /c "F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\cordova\build.bat" 
```
将此命令直接在问题机器中尝试:
```
 D:\k12student\k12student>cmd "/s /c "D:\k12student\k12student\\platforms\android\cordova\build.bat
ANDROID_HOME=C:\adt-bundle-windows-x86-20130917\sdk
JAVA_HOME=C:\Program Files\Java\jdk1.7.0_79
Running: D:\k12student\k12student\platforms\android\gradlew cdvBuildDebug -b D:\k12student\k12student\platforms\android\build.gradle -Dorg.gradle.daemon=true
Download https://repo1.maven.org/maven2/com/google/guava/guava/17.0/guava-17.0.jar
Download https://repo1.maven.org/maven2/net/sf/kxml/kxml2/2.3.0/kxml2-2.3.0.jar
Download https://repo1.maven.org/maven2/com/android/tools/layoutlib/layoutlib-api/24.0.0/layoutlib-api-24.0.0.jar
Download https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.1.1/httpclient-4.1.1.jar
Download https://repo1.maven.org/maven2/org/apache/httpcomponents/httpmime/4.1/httpmime-4.1.jar
Download https://repo1.maven.org/maven2/com/android/tools/dvlib/24.0.0/dvlib-24.0.0.jar
Download https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.8.1/commons-compress-1.8.1.jar
> Configuring > 0/2 projects > root project > 303 KB/356 KB downloaded 
```
执行正常,但在下载编译工具`gradle`的依赖. 对于更新慢完善有文章,可离线安装(也可等待完成):
`ionic build android error when download gradle - Stack Overflow`
http://stackoverflow.com/questions/29874564/ionic-build-android-error-when-download-gradle

完成后或重新执行build:
```
cmd "/s /c "F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\cordova\build.bat" 
```
apk包可正常打出来.

遗留的问题, ionic build android 仍旧执行空白
```
F:\work\wosai\code.wosai\edu.k12\k12student\k12student>ionic build android

F:\work\wosai\code.wosai\edu.k12\k12student\k12student>
```
雷同问题:
```
 F:\work\wosai\code.wosai\edu.k12\k12student\k12student>ionic run android
Running command: "C:\Program Files\nodejs\node.exe" F:\work\wosai\code.wosai\edu.k12\k12student\k12student\hooks\after_prepare\010_add_platform_class.js F:\work\wosai\code.wosai\edu.k12\k12student\k12student
add to body class: platform-android
Running command: cmd "/s /c "F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\cordova\run.bat""
ANDROID_HOME=G:\IDE\adt-bundle\sdk
JAVA_HOME=C:\Program Files\Java\jdk1.8.0_66 
```
关键:
```
cmd "/s /c "F:\work\wosai\code.wosai\edu.k12\k12student\k12student\platforms\android\cordova\run.bat""
```