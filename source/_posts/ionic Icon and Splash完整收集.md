title: ionic Icon and Splash完整收集
date: 2016-09-14 00:00:00
tags: [ionic]
 
---


# ionic Icon and Splash Screen Image Generation完整收集



---
# ionic Icon and Splash Screen Image Generation官方说明

- 使用命令（支持png、psd和ai，尺寸越大越好）
```
$ ionic resources --icon
$ ionic resources --splash
$ ionic resources
$ ionic resources ios
$ ionic resources --default
$ ionic resources --default --force
```


- 使用PSD模板

     -  [Photoshop Splash Screen Template](http://code.ionicframework.com/resources/splash.psd)

     - [Photoshop Icon Template](http://code.ionicframework.com/resources/icon.psd)



https://github.com/driftyco/ionic-cli#icon-and-splash-screen-image-generation


`介绍 博客`
http://blog.ionic.io/automating-icons-and-splash-screens/


`自定义图标 Customize Icons`
http://cordova.apache.org/docs/en/dev/config_ref/images.html#Icons%20and%20Splash%20Screens_


---
# 在线生成工具
`图标工场 - 移动应用图标生成工具，一键生成所有尺寸的应用图标`  ★ ★
http://icon.wuruihong.com/


`在线批量生成各种尺寸iOS、Android APP图表集合 - aTool在线工具`
http://www.atool.org/ios_logo.php


---
# 开源工具
`cordova-icon-splash-generator`
https://github.com/collingreen/cordova-icon-splash-generator


`busterc/cordova-resource-generators` - liunx,mac `35star`
https://github.com/busterc/cordova-resource-generators


`gulp-cordova-resources`
https://github.com/david-alexander/gulp-cordova-resources



`cordova-ionic-maven-archetype | cordova-icon-splash 生成器` ★
https://github.com/zhoujianhui/cordova-ionic-maven-archetype/blob/52d5186c785d1d27d06336916a2056c6c15d4980/test-cordova-ionic-maven-archetype/icon-splash/README.md


` cordova-splash `  `207star`  ★
https://github.com/AlexDisler/cordova-splash


`AlexDisler/cordova-icon` 404star  ★ ★ ` for Android and iOS, at least 192*192px (512*512px recommended) `
https://github.com/AlexDisler/cordova-icon
```
cordova-icon --config=config.xml --icon= resources/ icon.png
```


`AlexDisler/cordova-splash` 207star  ★ ★ ` Create a splash screen (2208x2208)`
https://github.com/AlexDisler/cordova-splash

```
cordova-splash --config=config.xml --splash=resources/splash.png

```
(生成后直接应用于platform中,需要手动取出.按规格存放resources)


`Icon-Splash-Resize`
https://github.com/dericeira/Icon-Splash-Resize



`github搜索:cordova splash 生成`
https://github.com/search?utf8=%E2%9C%93&q=cordova+splash+%E7%94%9F%E6%88%90&type=Code&ref=searchresults


---
`ionic框架iOS所需icon与splash规格（命名与尺寸）`
http://blog.leanote.com/freemem


`App各种Icon及Launch image的尺寸和用途 移动端尺寸基础知识`

https://my.oschina.net/u/2252309/blog/402724



---


# 文章介绍
`Cordova 3.x 基础（2） -- 应用图标icon和启动页面SplashScreen - Fish Where The Fish Are - ITeye技术网站`
http://rensanning.iteye.com/blog/2017380
```
如果你本地安装了ImageMagick，CLI还会自动调用ImageMagick来做成不同尺寸的图像。
需要注意的是：
（1）图像的路径是相对于工程根目录不是根据www
（2）不要和PhoneGap CLI的设置混淆
```
 
`Ionic Framework的一些命令行解释 - 推酷`
http://www.tuicool.com/articles/FFfiquu


`【Ionic】图标-名称-启动画修改_百度经验`
http://jingyan.baidu.com/article/fdffd1f85ee24ef3e88ca164.html


`【Ionic】App应用名字+应用图标+启动画的修改_百度经验`
http://jingyan.baidu.com/article/c35dbcb0ec59438916fcbcaf.html


