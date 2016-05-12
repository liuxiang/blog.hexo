title: ionic ios build error
date: 2016-3-3 00:00:00
categories:   mac

tags: [ ionic   ,  build ]


---
#  ionic ios build error: 插件(` IonicKeyboard` )无法编译
```
The following build commands failed:
CompileC build/赛学霸.build/Debug-iphonesimulator/赛学霸.build/Objects-normal/i386/IonicKeyboard-A84D2526EC939C9A.o 赛学霸/Plugins/com.ionic.keyboard/IonicKeyboard.m normal i386 objective-c com.apple.compilers.llvm.clang.1_0.compiler
(1 failure)
ERROR building one of the platforms: Error code 65 for command: xcodebuild with args: -xcconfig,/Users/liuxiang/Desktop/svnWork/k12student_ios/platforms/ios/cordova/build-debug.xcconfig,-project,赛学霸.xcodeproj,ARCHS=i386,-target,赛学霸,-configuration,Debug,-sdk,iphonesimulator,build,VALID_ARCHS=i386,CONFIGURATION_BUILD_DIR=/Users/liuxiang/Desktop/svnWork/k12student_ios/platforms/ios/build/emulator,SHARED_PRECOMPS_DIR=/Users/liuxiang/Desktop/svnWork/k12student_ios/platforms/ios/build/sharedpch
You may not have the required environment or OS to build this project
Error: Error code 65 for command: xcodebuild with args: -xcconfig,/Users/liuxiang/Desktop/svnWork/k12student_ios/platforms/ios/cordova/build-debug.xcconfig,-project,赛学霸.xcodeproj,ARCHS=i386,-target,赛学霸,-configuration,Debug,-sdk,iphonesimulator,build,VALID_ARCHS=i386,CONFIGURATION_BUILD_DIR=/Users/liuxiang/Desktop/svnWork/k12student_ios/platforms/ios/build/emulator,SHARED_PRECOMPS_DIR=/Users/liuxiang/Desktop/svnWork/k12student_ios/platforms/ios/build/sharedpch
```


# 原因
安装插件时的cordova版本与当前的cordova版本不一致.


# 解决办法


`明确原因`: 安装重复 `com.ionic.keyboard` 和 `ionic-plugin-keyboard`

`ionic-plugin-keyboard` 会自动添加(可能是判断` com.ionic.keyboard `版本是否过旧)
我是将 `com.ionic.keyboard`删除来解决此问题的.


方法一.当前cordova -v 6.0.0 回退到安装插件时的cordova -v 5.3.3即可

```
npm remove cordova
npm add cordova@5.3.3
```


方法二.使用新版本cordova卸载重装所有插件
```
# 完整
com.seed.cordova.huhu 0.0.2 "Huhu"
com.wosaiteach.cordova.openapp 0.2.16 "OpenApp"


cn.jpush.phonegap.JPushPlugin 2.1.0 "JPush Plugin"
com.ionic.keyboard 1.0.4 "Keyboard"
com.qdc.plugins.alipay 1.0.0 "Alipay"
cordova-plugin-app-version 0.1.8 "AppVersion"
cordova-plugin-appavailability 0.4.2 "AppAvailability"
cordova-plugin-camera 1.2.0 "Camera"
cordova-plugin-device 1.1.0 "Device"
cordova-plugin-inappbrowser 1.1.0 "InAppBrowser"
cordova-plugin-payment-iap 2.0.44 "Cordova IAP plugin"
cordova-plugin-qqsdk 0.3.9 "YCQQ"
cordova-plugin-splashscreen 3.0.0 "Splashscreen"
cordova-plugin-whitelist 1.2.0 "Whitelist"
ionic-plugin-keyboard 1.0.8 "Keyboard"
org.apache.cordova.file 1.3.3 "File"
org.apache.cordova.file-transfer 0.5.0 "File Transfer"
org.apache.cordova.network-information 0.2.15 "Network Information"
org.apache.cordova.statusbar 0.1.10 "StatusBar"
phonegap-plugin-barcodescanner 4.1.0 "BarcodeScanner"


# 手动拷贝(无网络管理)
cordova plugin add com.seed.cordova.huhu
cordova plugin add com.wosaiteach.cordova.openapp


# 自动
ionic state save  # 更新package.json
ionic state reset # 依据package.json 重装plugin
或
cordova plugin add cn.jpush.phonegap.JPushPlugin
cordova plugin add cordova-plugin-app-version
cordova plugin add cordova-plugin-appavailability
cordova plugin add cordova-plugin-camera
cordova plugin add cordova-plugin-payment-iap
cordova plugin add cordova-plugin-qqsdk
cordova plugin add org.apache.cordova.file 
cordova plugin add org.apache.cordova.file-transfer
cordova plugin add org.apache.cordova.network-information
cordova plugin add org.apache.cordova.statusbar
cordova plugin add phonegap-plugin-barcodescanner 
...
```


**参考**
`Build iOS failed - Cordova Ionic iOS`

http://stackoverflow.com/questions/32678036/build-ios-failed-cordova-ionic-ios


`Ionic build ios plugin errors (exit code 65) - ionic - Ionic Forum`

https://forum.ionicframework.com/t/ionic-build-ios-plugin-errors-exit-code-65/7274



` Ionic run ios failed (error 65) after successful build ios - ionic - Ionic Forum `
https://forum.ionicframework.com/t/ionic-run-ios-failed-error-65-after-successful-build-ios/12633/2


<!-- more -->
