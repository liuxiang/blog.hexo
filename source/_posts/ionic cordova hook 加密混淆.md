title:  ionic cordova hook 加密混淆
date: 2016-05-10 00:00:00
categories: 前端 加密 混淆

tags: [ 前端 加密 混淆 ， gulp， ionic ，cordova hook   ]


---
#　ionic cordova hook 加密混淆 & gulp[templatecache annot



* （cordova hook）[ jshint,async ] 检查javascript：这一步需要在代码压缩和代码混淆之前进行以保证javascript代码无错误

*   （gulp task）[ gulp-angular-templatecache ] 将html页面代码转换为angular的JS代码：这一步起到了混淆html页面代码的作用
*   （gulp task）[ gulp-ng-annotate ] 启用angular严格依赖注入：这一步需要在代码混淆之前进行以保证angular的依赖注入没有问题
*   （gulp task）[ gulp-useref ] 组合js代码以及组合css代码：这一步起到了混淆js代码以及css代码的作用
*   （cordova hook）[ uglify,mv ] 代码丑化、压缩、混淆


---
# 检查javascript



* 支持
`npm install jshint --save-dev &  npm install async --save-dev`
或 `cnpm install jshint --save-dev & c npm install async --save-dev`


* 网络获取cordova hooks文件，复制到$PROJECT_DIR/hooks/before_prepare文件夹里

`hooks\after_prepare\01_jshint.js `
` cordova hooks  ` http://www.ionic.ren/wp-content/uploads/2015/11/after_prepare.zip


* `验证` (注意：仅添加 `hooks\after_prepare\01_jshint.js`钩子验证 )
`ionic build android`


# 把html模板转换为angularjs模板
*  支持
`npm install gulp-angular-templatecache --save-dev`
或 `cnpm install gulp-angular-templatecache --save-dev`


*  gulpfile.js
```
var templateCache = require('gulp-angular-templatecache'); 
var paths = { 
  sass: ['./scss/**/*.scss'], 
  templatecache: ['./www/templates/**/*.html'] 
}; 
gulp.task('templatecache', function (done) { 
  gulp.src('./www/templates/**/*.html') 
    .pipe(templateCache({standalone:true, root: "templates"})) 
    .pipe(gulp.dest('./www/js')) 
    .on('end', done); 
}); 
gulp.task('default', ['sass', 'templatecache']); 
gulp.task('watch', function() { 
  gulp.watch(paths.sass, ['sass']); 
  gulp.watch(paths.templatecache, ['templatecache']); 
}); 
```


*   app.js
`angular.module('starter', ['ionic', 'starter.controllers', 'starter.services', 'templates'])`



*   验证
`gulp  templatecache `


* 补充依赖（实际缺少再装）
`cnpm install gulp-jshint --save-dev`
`cnpm install gulp-uglify --save-dev`
`cnpm install gulp-concat --save-dev`
`cnpm install gulp-minify-html --save-dev`
`cnpm install gulp-util --save-dev`
`cnpm install gulp-minify-css --save-dev`
`cnpm install gulp-rename --save-dev`
`cnpm install shelljs --save-dev`
`cnpm install gulp-ng-annotate --save-dev`
`cnpm install gulp-useref --save-dev`


# 启用angular ng-strict-di
*  支持

`npm install gulp-ng-annotate --save-dev`
或 `cnpm install gulp-ng-annotate --save-dev`


*  gulpfile .js
```
var ngAnnotate = require('gulp-ng-annotate'); 
var paths = { 
  sass: ['./scss/**/*.scss'], 
  templatecache: ['./www/templates/**/*.html'], 
  ng_annotate: ['./www/js/*.js'] 
}; 
gulp.task('ng_annotate', function (done) { 
  gulp.src('./www/js/*.js') 
    .pipe(ngAnnotate({single_quotes: true})) 
    .pipe(gulp.dest('./www/dist/dist_js/app')) 
    .on('end', done); 
}); 
gulp.task('default', ['sass', 'templatecache', 'ng_annotate']); 
gulp.task('watch', function() { 
  gulp.watch(paths.sass, ['sass']); 
  gulp.watch(paths.templatecache, ['templatecache']); 
  gulp.watch(paths.ng_annotate, ['ng_annotate']); 
});
```


*  index.html


```
<script src="dist/dist_js/app/app.js"></script> 
<script src="dist/dist_js/controllers.js"></script> 
<script src="dist/dist_js/services.js"></script> 
<script src="dist/dist_js/templates.js"></script> 
 
<body ng-app="starter" ng-strict-di>  
```


*  验证
`gulp  ng_annotate `


# 合并js文件以及css文件
* 支持
`npm install gulp-useref@2.1.0 --save-dev`

或 `cnpm install gulp-useref@2.1.0 --save-dev`


*  gulpfile .js

```
var useref = require('gulp-useref'); 
var paths = { 
  sass: ['./scss/**/*.scss'], 
  templatecache: ['./www/templates/**/*.html'], 
  ng_annotate: ['./www/js/*.js'], 
  useref: ['./www/*.html'] 
}; 
gulp.task('useref', function (done) { 
  var assets = useref.assets(); 
  gulp.src('./www/*.html') 
    .pipe(assets) 
    .pipe(assets.restore()) 
    .pipe(useref()) 
    .pipe(gulp.dest('./www/dist')) 
    .on('end', done); 
}); 
gulp.task('default', ['sass', 'templatecache', 'ng_annotate', 'useref']); 
gulp.task('watch', function() { 
  gulp.watch(paths.sass, ['sass']); 
  gulp.watch(paths.templatecache, ['templatecache']); 
  gulp.watch(paths.ng_annotate, ['ng_annotate']); 
  gulp.watch(paths.useref, ['useref']); 
});  
```


* index.html



```
<!-- build:css dist_css/styles.css --> 
<link href="css/style.css" rel="stylesheet"> 
<link href="css/ionic.app.css" rel="stylesheet"> 
<!-- endbuild --> 
 
<!-- build:js dist_js/app.js --> 
<script src="dist/dist_js/app/app.js"></script> 
<script src="dist/dist_js/app/controllers.js"></script> 
<script src="dist/dist_js/app/services.js"></script> 
<script src="dist/dist_js/app/templates.js"></script> 
<!-- endbuild -->  
```


* 验证
`gulp useref`


---
# Cordova Hook 
* 支持
`npm install cordova-uglify --save-dev &  npm instal mv --save-dev`
或 `cnpm install cordova-uglify --save-dev & cnpm instal mv --save-dev`





* 网络获取cordova hooks文件，复制到$PROJECT_DIR/hooks/before_prepare文件夹里

>  hooks\after_prepare\ 020_remove_sass_from_platforms.js
hooks\after_prepare\ 030_clean_dev_files_from_platforms.js
hooks\after_prepare\ 040_move_dist_files_to_platforms.js
hooks\after_prepare\ 050_clean_obfuscation.js
hooks\after_prepare\ 060_uglify.js


* 验证
`ionic build android/ios`



*  补充依赖 （实际缺少再装）
`cnpm install uglify-js --save-dev` -- 060_uglify.js
`cnpm install clean-css@2.2.22 --save-dev` -- 060_uglify.js
`cnpm install ng-annotate --save-dev` -- 060_uglify.js


*  结果： platforms\android\build\outputs\apk\ `android-debug.apk` 混淆效果
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-5-10/52737274.jpg)



![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-10/91724998.jpg)


---


#  ` gulp-angular-templatecache `插件效果
*  对html页面代码的混淆是将html页面代码处理成angular的js代码（保存到一个js文件中）
```
angular.module("templates", []).run(["$templateCache", function($templateCache) {$templateCache.put
$templateCache.put("templates/tab-account.html","<ion-view view-title=\"Account\">\n  <ion-content>
$templateCache.put("templates/tab-chats.html","<ion-view view-title=\"Chats\">\n  <ion-content>\n 
$templateCache.put("templates/tab-dash.html","<ion-view view-title=\"Dashboard\">\n  <ion-content c
$templateCache.put("templates/tabs.html","<!--\nCreate tabs with an icon and label, using the tabs-
$templateCache.put("templates/a/chat-detail.html","<!--\n  This template loads for the \'tab.friend
```
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-10/61352120.jpg)


# `gulp-ng-annotate`插件效果(启用angular ng-strict-di)
```
.controller('ChatDetailCtrl', function($scope, $stateParams, Chats)
```
```
.controller('ChatDetailCtrl', ['$scope', '$stateParams', 'Chats', function($scope, $stateParams, Chats)
```
> 启用angular ng-strict-di  
在我们进行代码压缩之前，我们需要启用angular的ng-strict-di，即严格依赖注入，使用ng-strict-di使得工程中依赖注入不会有问题，更多关于ng-strict-di可以看这里。


# `gulp-useref` 插件效果( 合并js文件以及css文件 ) & 时常配合 ` gulp.watch `一起使用
效果：
```
    <!-- build:css dist_css/styles.css -->
    <link href="css/style.css" rel="stylesheet">
    <link href="css/ionic.app.css" rel="stylesheet">
    <!-- endbuild -->


    <!-- your app's js -->
    <!-- build:js dist_js/app.js -->
    <script src="dist/dist_js/app/app.js"></script>
    <script src="dist/dist_js/app/controllers.js"></script>
    <script src="dist/dist_js/app/services.js"></script>
    <script src="dist/dist_js/app/templates.js"></script>
    <!-- endbuild -->
```
```
    <link rel="stylesheet" href="dist_css/styles.css">



    <!-- your app's js -->
    <script src="dist_js/app.js"></script>
```
---
问题：platforms\android\assets\www\dist_css\styles.css 经过`060_uglify.js`后内容消失
显示：`[object Object]`


原因：依据代码引用，因` clean-css `插件问题
最大支持版本clean-css@2.2.22，以上都会出此错误.(或了解新版本，使用新函数)
https://github.com/jakubpawlowicz/clean-css/blob/master/History.md


---
`源码`  https://github.com/liuxiang/ionic-demo-gulp.git

`附件` <见源码>


---
**参考**
`ionic代码压缩与代码混淆 - 菜鸟的学习之路 - 博客频道 - CSDN.NET`
http://blog.csdn.net/u010730126/article/details/50115579


`如何制作一个发布版的ionic应用？`
http://rensanning.iteye.com/blog/2205322


`Production ready apps with ionic framework`
https://www.airpair.com/ionic-framework/posts/production-ready-apps-with-ionic-framework


<!-- more -->
