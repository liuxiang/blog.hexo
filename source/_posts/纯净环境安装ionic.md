title: 纯净环境安装ionic
date: 2016-2-20 00:00:00 #发表日期，一般不改动
categories: ionic #文章文类
tags: [ionic, ionic安装]

---
# 1.安装node
https://nodejs.org/en/download/releases/

# 2.安装cordova,ionic & 创建工程
`npm install -g cordova ionic`
`ionic start myApp tabs`

# 3.ionic添加`platform(平台)`, 出错-缺少android-SDK支持
`ionic platform add android`

## 3.1为编译android端安装文件(apk),初始化android环境,安装android-sdk
安装android-sdk `android-sdk_r24.4.1-windows.zip`
http://tools.android-studio.org/index.php/sdk

需要配置环境变量:
```
ANDROID_HOME=G:\IDE\adt-bundle\sdk
set Path=%Path%;%ANDROID_HOME%\tools;%ANDROID_HOME%\platform-tools
```

# 4.ionic添加`platform(平台)`
`ionic platform add android`

# 5.编译代码
`ionic build android`

可能出现问题一:
```
Downloading http://services.gradle.org/distributions/gradle-2.2.1-all.zip
...
org.gradle.wrapper.ExclusiveFileAccessManager.access(ExclusiveFileAccessManager.java:78
```

原因: 资源被墙,下载超时.
解决: 手动下载,修改下载目标位置
         或
```
新增环境变量(路径需转义)：
CORDOVA_ANDROID_GRADLE_DISTRIBUTION_URL
distributionUrl=D:\tool/gradle-2.2.1-all.zip
```
参考:
http://stackoverflow.com/questions/29874564/ionic-build-android-error-when-download-gradle
http://blog.csdn.net/mzwang123/article/details/22280825

# 6.运行项目平台到设备(手机)
`ionic runandroid`

可能出现问题:
` HINT: For a faster emulator, use an Intel System Image and install the HAXM device driver`
原因:设备android内核版本太低
解决:sdk安装更低的支持 `android-21` `android-22` `android-23`
参考:
http://stackoverflow.com/questions/29487513/android-sdk-error-even-after-installing-it-ionic-framework

<!-- more -->