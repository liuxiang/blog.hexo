title:  yeoman 之   generator-gulp-angular 自定义lib-js引入

date: 2017-06-07 00:00:00
tags: [yeoman ]



---


# 增加自定义lib-js依赖
- index.html
```
<!-- build:js(src) scripts/vendor.js -->
<!-- bower:js -->
<!-- run `gulp inject` to automatically populate bower script dependencies -->
<!-- endbower -->
<script src="lib/qrcodejs/qrcode.js"></script><!-- 二维码 -->
<!-- endbuild -->


<!-- build:js({.tmp/serve,.tmp/partials,src}) scripts/app.js -->
<!-- inject:js -->
<!-- js files will be automatically insert here -->
<!-- endinject -->


<!-- inject:partials -->
<!-- angular templates will be automatically converted in js and inserted here -->
<!-- endinject -->
<!-- endbuild -->
```


---
`Web-developer's notes`
http://eugenioz.blogspot.com/

```
Now add links to JS files into first build:js block, after endbower and before endbuild line, so it becomes:


    <!-- build:js(src) scripts/vendor.js -->
    <!-- bower:js -->
    <!-- run `gulp inject` to automatically populate bower script dependencies -->
    <!-- endbower -->
    <script src="../bower_components/ionic/release/js/ionic.js"></script>
    <script src="../bower_components/ionic/release/js/ionic-angular.js"></script>
    <!-- endbuild -->
```


- 更多：
`gulp-inject`
https://www.npmjs.com/package/gulp-inject
```
  <!-- inject:js -->
  <script src="/src/lib1.js"></script> 
  <script src="/src/lib2.js"></script> 
  <!-- endinject -->
```
