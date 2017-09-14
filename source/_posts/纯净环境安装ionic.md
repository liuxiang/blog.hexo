title: 纯净环境安装ionic
date: 2016-2-20 00:00:00 #发表日期，一般不改动
categories:  ionic  #文章文类
tags: [ ionic ,  ionic安装 ]


---

# 1.安装node

https://nodejs.org/en/download/releases/


#   2.安装cordova,ionic & 创建工程
`npm install -g cordova ionic`
`ionic start myApp tabs`


`更多 ionic start` 
http://www.ionic.wang/start-index.html

https://github.com/driftyco/ionic-cli



#  3 .ionic添加`platform(平台)`,  出错-缺少android-SDK支持

`ionic platform add android`或`cordova platform add android`


## 3.1为编译android端安装文件(apk),初始化android环境,安装android-sdk
安装android-sdk `android-sdk_r24.4.1-windows.zip`
http://tools.android-studio.org/index.php/sdk


需要配置环境变量:
```
ANDROID_HOME= G:\IDE\adt-bundle\sdk
set  Path=%Path%;% ANDROID_HOME %\tools;% ANDROID_HOME %\ platform-tools
```


# 4.ionic添加`platform(平台)`

`ionic platform add android`



#   5.编译代码
`ionic build android`


可能出现问题一:
```
Downloading http://services.gradle.org/distributions/gradle-2.2.1-all.zip
...
org.gradle.wrapper.ExclusiveFileAccessManager.access(ExclusiveFileAcc essManager.java:78
```


原因: 资源被墙,下载超时.

解决: 手动下载
http://services.gradle.org/distributions
( gradle-2.2.1-all.zip下载很慢时，可以下载最新包重命名即可 )


直接放置目标位置
```

# 放入此目录下
C:\Users\Administrator\.gradle\wrapper\dists\gradle-2.2.1-all\****\

```
或
```
新增环境变量(路径需转义)：
CORDOVA_ANDROID_GRADLE_DISTRIBUTION_URL
distributionUrl=D:\tool/gradle-2.2.1-all.zip
```
参考:
http://stackoverflow.com/questions/29874564/ionic-build-android-error-when-download-gradle
http://blog.csdn.net/mzwang123/article/details/22280825


`卡包`
```
Download https://repo1.maven.org/maven2/org/antlr/antlr/3.5.2/antlr-3.5.2.jar

```




问题二:
```
D:\workspace\edu.k12\k12teacher>ionic build android
Running command: D:\node\node.exe D:\workspace\edu.k12\k12teacher\hooks\after_prepare\010_add_platfo
rm_class.js D:\workspace\edu.k12\k12teacher
add to body class: platform-android
ERROR building one of the platforms: Please install Android target: "android-23".
 
Hint: Open the SDK manager by running: "D:\android-sdk-windows\tools\android.bat"
You will require:
1. "SDK Platform" for android-23
2. "Android SDK Platform-tools (latest)
3. "Android SDK Build-tools" (latest)
You may not have the required environment or OS to build this project
Error: Please install Android target: "android-23".
 
Hint: Open the SDK manager by running: "D:\android-sdk-windows\tools\android.bat"
You will require:
1. "SDK Platform" for android-23
2. "Android SDK Platform-tools (latest)
3. "Android SDK Build-tools" (latest)
```
处理办法:
方法一:使用下载工具(可能要翻墙,不大-挺快) `adt-bundle\SDK Manager.exe`

![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/89742526.jpg)


方法二:网络下载放置 `adt-bundle\sdk\platforms`下
方法三:android-**降级,更新文件`\platforms\android\project.properties`
更新C盘设置,用于下次新添加工程使用指定的版本
`C:\Users\Administrator\.cordova\lib\npm_cache\cordova-android\4.1.1\package\framework\project.properties`



# 6.运行项目平台到设备(手机)
`ionic runandroid`



可能出现问题:
` HINT: For a faster emulator, use an Intel System Image and install the HAXM device driver `
原因:设备android内核版本太低
解决:sdk安装更低的支持 `android-21`  `android-22`  `android-23`
参考:
http://stackoverflow.com/questions/29487513/android-sdk-error-even-after-installing-it-ionic-framework



<!-- more -->
