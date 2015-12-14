title: ionic打包未签名apk
date: 2015-12-14 00:00:05 #发表日期，一般不改动
categories: ionic#文章文类
tags: [ionic,android打包]

---
# 常规打包`cordova build --release android`
可能会出现的错误
![](http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/77923767.jpg)

查看错误日志发现问题基本是 translation 错误
# 解决方法
在 platforms/android/build.gradle 文件的android {} 节点中增加
```
lintOptions{
        abortOnError false
        checkReleaseBuilds false
}
```
如图:
![](http://7xnbs3.com1.z0.glb.clouddn.com/15-12-14/57618196.jpg)

<!-- more -->

# 参考
`Error when running cordova build –release android - Stack Overflow`
http://stackoverflow.com/questions/30345879/error-when-running-cordova-build-release-android

`Translations errors in cordova 5 · Issue #66 `
https://github.com/wymsee/cordova-imagePicker/issues/66
