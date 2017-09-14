title:  前端 加密 混淆  & gulp
date: 2016-05-09 00:00:00
categories:  前端 加密 混淆
tags: [ 前端 加密 混淆 ， gulp ]


---


# 混淆工具：  
* YUI Compressor
*   Google Closure Compiler
*   UglifyJS
*   JScrambler (付费) - 目前还无法被反混淆
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-10/99220276.jpg)


# 反混淆工具：  
*   jsbeautifier.org
*   JSDetox


**参考** `技术分享：几种常见的JavaScript混淆和反混淆工具分析实战`

http://www.tuicool.com/articles/ZNfa6nz


`Javascript代码压缩细节 - 推酷` `解读uglifyJS`
http://www.tuicool.com/articles/jmIB3a


---


```
压缩css(gulp-minify-css)
合并文件(gulp-concat)
js代码校验(gulp-jshint)
合并js文件(gulp-concat)
压缩js代码(gulp-uglify)
less编译(gulp-less)
sass编译(gulp-sass)
自动添加css前缀(gulp-autoprefixer)
压缩图片(gulp-imagemin)
自动刷新页面(gulp-livereload)
图片缓存，只有图片替换了才压缩(gulp-cache)
更改提醒(gulp-notify)
```
```
var del = require('del'),
    gulp = require('gulp'),
    concat = require('gulp-concat'),
    jshint = require('gulp-jshint'),
    minifycss = require('gulp-minify-css'),
    rev = require('gulp-rev'),
    uglify = require('gulp-uglify'),
    sourcemaps = require('gulp-sourcemaps'),
    revCollector = require('gulp-rev-collector-r'),
    runSequence = require('run-sequence');
```
`gulp使用：进行压缩合并js、css - 推酷`

http://www.tuicool.com/articles/3EVNraf


`gulp压缩合并静态文件 - 推酷`
http://www.tuicool.com/articles/aae6vi



---



```
gulp-sourcemaps 生成source map，方便跟踪问题代码
gulp-concat 合并文件
gulp-uglify 压缩js
gulp-minify-css 压缩css
gulp-rev 为文件加上版本控制信息
gulp-rev-collector 配合gulp-rev替换文件名
gulp-jshint js代码检查
run-sequence 顺序执行task
```
`gulp压缩合并静态文件 - 推酷`
http://www.tuicool.com/articles/aae6vi


`使用Gulp进行Javascript以及css压缩合并 - 推酷`
http://www.tuicool.com/articles/IZR3AfV



---


* （cordova hook）[jshint,async] 检查javascript：这一步需要在代码压缩和代码混淆之前进行以保证javascript代码无错误
* （gulp task）[gulp-angular-templatecache] 将html页面代码转换为angular的JS代码：这一步起到了混淆html页面代码的作用
* （gulp task）[gulp-ng-annotate] 启用angular严格依赖注入：这一步需要在代码混淆之前进行以保证angular的依赖注入没有问题
* （gulp task）[gulp-useref] 组合js代码以及组合css代码：这一步起到了混淆js代码以及css代码的作用
* （cordova hook）[uglify,mv] 代码丑化、压缩、混淆


`cordova hook`
npm install jshint --save-dev
npm install async --save-dev
npm install gulp-angular-templatecache --save-dev
npm install gulp-ng-annotate --save-dev
npm install gulp-useref --save-dev
npm install cordova-uglify --save-dev
npm instal mv --save-dev


**参考** `ionic代码压缩与代码混淆 - 菜鸟的学习之路 - 博客频道 - CSDN.NET`
http://blog.csdn.net/u010730126/article/details/50115579


---
`如何制作一个发布版的ionic应用？`
http://rensanning.iteye.com/blog/2205322


`myApp.rar (3.9 MB) `

http://dl2.iteye.com/upload/attachment/0107/8283/37f71219-d435-3413-bb36-4e1df3c86df9.rar


`before-after-apk.rar (4.3 MB)`

http://dl2.iteye.com/upload/attachment/0107/8300/86187ff7-c063-34af-986b-d2d6cb36e842.rar




`ionic实用功能(二)——制作一个发布版的ionic应用(混淆、打包) | 志慧师兄`
http://www.ionic.ren/2015/11/25/ionic%E5%AE%9E%E7%94%A8%E5%8A%9F%E8%83%BD%E4%BA%8C-%E5%88%B6%E4%BD%9C%E4%B8%80%E4%B8%AA%E5%8F%91%E5%B8%83%E7%89%88%E7%9A%84ionic%E5%BA%94%E7%94%A8%E6%B7%B7%E6%B7%86%E3%80%81/
(同上篇，仅多`after_prepare`hook文件下载)
`after_prepare`
http://www.ionic.ren/wp-content/uploads/2015/11/after_prepare.zip


<!-- more -->
