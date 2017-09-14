title: ionic-native-transitions 试用
date: 2016-12-16 00:00:03
tags: [ionic-native-transitions]


---
`github` 
https://github.com/shprink/ionic-native-transitions


---
# 1.安装`node_modules`
```
npm install ionic-native-transitions --save

```


# 2.安装lib库依赖
```
bower install shprink/ionic-native-transitions

```
- 引入
```
<script src="./PathToBowerLib/dist/ionic-native-transitions.min.js"></script>

```


# 3.安装cordova插件plugin
```
# Using Cordova
cordova plugin add https://github.com/Telerik-Verified-Plugins/NativePageTransitions#0.6.5
 
# Using Ionic CLI
ionic plugin add https://github.com/Telerik-Verified-Plugins/NativePageTransitions#0.6.5
```
- IOS9设备 闪屏问题，安装插件解决
```
# Using Cordova
cordova plugin add cordova-plugin-wkwebview
 
# Using Ionic CLI
ionic plugin add cordova-plugin-wkwebview
```


# 4.ionic（angular）模块引入. `app.js `
```
angular.module('yourApp', [ 'ionic-native-transitions' ]);
```


# 5.安装尝试(无需改变默认配置)
```
ionic platform add android
ionic run android
```


---
# 6.修改默认配置
```
.config(function($ionicNativeTransitionsProvider){
    nativeTransitionConfig();//  场景切换,配置
});
```
```
/**
* 场景切换,配置
*/
function nativeTransitionConfig() {
  $ionicNativeTransitionsProvider.setDefaultOptions({
    duration: 200, // in milliseconds (ms), default 400,
    slowdownfactor: 4, // overlap views (higher number is more) or no overlap (1), default 4
    iosdelay: -1, // ms to wait for the iOS webview to update before animation kicks in, default -1
    androiddelay: -1, // same as above but for Android, default -1
    winphonedelay: -1, // same as above but for Windows Phone, default -1,
    fixedPixelsTop: 0, // the number of pixels of your fixed header, default 0 (iOS and Android)
    fixedPixelsBottom: 0, // the number of pixels of your fixed footer (f.i. a tab bar), default 0 (iOS and Android)
    triggerTransitionEvent: '$ionicView.afterEnter', // internal ionic-native-transitions option
    backInOppositeDirection: false // Takes over default back transition and state back transition to use the opposite direction transition to go back
  });
 
  /**
   * transitions
   * Slide 滑动
   * Flip 翻转
   * Fade 褪色
   * Drawer 抽屉
   * Curl 卷曲
   */
  $ionicNativeTransitionsProvider.setDefaultTransition({
    type: 'slide',
    direction: 'left'
  });
 
  $ionicNativeTransitionsProvider.setDefaultBackTransition({
    type: 'slide',
    direction: 'right'
  });
 
  /**
   * 更多配置,详见官方介绍：
   * https://github.com/shprink/ionic-native-transitions
   */
}
```


---
**参考**
`ionic-native-transitions让你的Ionic应用比原生还快`

http://www.phonegap100.com/article-482-1.html
```

ionic-native-transitions调用原生页面切换实现ionic路由切换，让你的ionic应用比原生的应用更快
```
---
`ionic react-native和native开发移动app那个好`
http://www.phonegap100.com/article-486-1.html
```
ionic使用ionic-native-transitions   调用原生专场基本看不出和原生区别  （适合android ios）

```


`让ionic应用实现类原生app的平滑过渡 | 常亮的技术博客`
http://www.diantuo.net/1613

