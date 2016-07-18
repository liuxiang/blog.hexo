title: ionic 之 package.json
date: 2016-1-7 00:00:00 #发表日期，一般不改动
categories: ionic  #文章文类
tags: [ionic ,package.json ]



---
# `ionic state save` 更新 package.json

> 会依据` plugins `的实际安装情况,更新`package.json`中的` cordovaPlugins `项


```
Saving your Ionic app state of platforms and plugins
Saved platform
Plugin cordova-plugin-file does not exist in the plugins directory. Skipping
Saved plugins
Saved package.json
```


# `ionic state restore` 恢复及安装

> 依据 package.json文件中的 cordovaPlugins和cordovaPlatforms属性.来恢复应用程序的 plugins 和 platforms.
可以手工填充 cordovaPlugins和cordovaPlatforms属性,再使用 `ionic state restore`恢复安装


```
F:\work\wosai\code.wosai\edu.k12\k12student>ionic state restore
Attempting to restore your Ionic application from package.json


Restoring Platforms


cordova platform add android


Restore platforms is complete


Restoring Plugins


cordova plugin add cordova-plugin-device
cordova plugin add cordova-plugin-whitelist
cordova plugin add cordova-plugin-splashscreen
cordova plugin add com.ionic.keyboard
cordova plugin add org.apache.cordova.file
cordova plugin add org.apache.cordova.network-information
cordova plugin add D:\ionic\k12student\AlipayPluginAndJPushPlugin\cordova-qdc-alipay-master
cordova plugin add org.apache.cordova.statusbar
cordova plugin add D:\cordova\phonegap-plugin-barcodescanner-master
cordova plugin add D:\cordovaPlugin\jpush-phonegap-plugin-master
cordova plugin add https://github.com/whiteoctober/cordova-plugin-app-version.git
cordova plugin add cordova-plugin-camera
cordova plugin add D:\ionic\k12student\cordova-plugin-huhu
cordova plugin add cordova-plugin-crosswalk-webview
cordova plugin add D:\ionic\k12student\cordova-plugin-file-transfer-r0.5.0
Restore plugins is complete


Ionic state restore completed


```


# `ionic state clear` 清理插件(慎用)
> <font color=red>
慎用,部分本地插件删除后无法从网络中下载.其次package.json的相关plugins配置一并清理,丢失了平台对插件的依赖
</font>
> 删除 ` package.json ` cordovaPlugins, cordovaPlatforms 同时删除工程文件夹`p lugins,platforms `


```
Clearing out your Ionic app of platforms, plugins, and package.json entries
Ionic app state cleared
```


# ` ionic state reset ` 重置 (慎用)
>  <font color=red> 重置方法首先将删除您的平台和插件文件夹. </font>
然后会依据你的package.json文件指定的 plugins 和 platforms 重新安装


```
F:\work\wosai\code.wosai\edu.k12\k12student>ionic state reset
Removed platforms and plugins
Attempting to restore your Ionic application from package.json
 
Restoring Platforms
 
 
Restore platforms is complete
 
Restoring Plugins
 
Restore plugins is complete
 
Ionic state restore completed
 
Ionic reset state complete
```


**参考**
`driftyco/ionic-cli`

https://github.com/driftyco/ionic-cli



<!-- more -->
