title: crosswalk Android ADT打包(ionic打包正常),运行闪退问题解决
date: 2015-12-05 00:00:00 #发表日期，一般不改动
categories: Crosswalk  #文章文类 
tags: [Crosswalk , Android,ionic ]

---
# 日志
```
could not find class 'org.xwalk.vore.XWalkActiveitDelegate',referenced form ...



06-21 10:12:52.495: E/Trace(2682): error opening trace file: No such file or directory (2)
06-21 10:12:52.723: E/AndroidRuntime(2682): FATAL EXCEPTION: main
06-21 10:12:52.723: E/AndroidRuntime(2682): java.lang.RuntimeException: Unable to start activity ComponentInfo{com.**.MainActivity}: java.lang.RuntimeException: Failed to create webview. 
```
>日志体现 xwalk未引入.运行时异常,无法创建webview


## 反编译apk安装包内容查看

`引用Crosswalk,ADT打包,会出现黑屏的apk`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-5/44405151.jpg)


`引用Crosswalk,ionic打包,正常apk`

![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-5/32981252.jpg)


>发现差异,缺少`org.xwalk.core`支持代码


# 解决办法
## 1.下载Crosswalk官网资源包, Cordova Android (ARM)
https://crosswalk-project.org/documentation/downloads.html


## 2. 增加项目library
`准备工作:`ionic build android , 更新platforms


`操作:`
`crosswalk-cordova-15.44.384.12-arm\framework\xwalk_core_library` 
拷贝至
`<project>\platforms\android\xwalk_core_library`


## 3.更新项目配置,使用ADT
ADT导入library项目` xwalk_core_library `,更新` xwalk_core_library_java.jar `引用
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-5/54973604.jpg)


更新主工程`MainAvtivity`,引入library project ` xwalk_core_library `
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-5/498019.jpg)


## 4.运行
Run as : Android Application



## 5.反编译apk安装包内容查看
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-5/32981252.jpg)


# 注意:如果要还原ionic打包
需要回退第三步, 断开自己添加的librafy porject  ` xwalk_core_library ` ,否则会与ionic自身结构的`xwalk`冲突
```
FAILURE: Build failed with an exception.


* What went wrong:
Execution failed for task ':processArmv7DebugResources'.
> Error: more than one library with package name 'org.xwalk.core'
You can temporarily disable this error with android.enforceUniquePackageName=false
However, this is temporary and will be enforced in 1.0


* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.


BUILD FAILED
```


#  理想状态: ionic可打出包含`Crosswalk`插件的无签名的apk包
见文章`ionic 打包未签名apk`
<!-- more -->
