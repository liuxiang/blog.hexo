title:  yeoman 之 generator-gulp-angular 自动注入js


date: 2015-12-28 00:00:00
tags: [yeoman ]



---


`generator-gulp-angular工程的Gulp任务介绍`
https://github.com/Swiip/generator-gulp-angular/blob/139e2322da3ee2e4be47cbf8fd0beae047c1c41e/generators/app/templates/gulp/README.md


inject.js
```
Inject task which link project files in the index.html and write the result in .tmp/serve/index.html
 
Project CSS files
Project JS files
Bower css and js deps
Warning The src/index.html is not modified (it was the case in previous version and is still the case in other generators) but the injected index.html is placed in .tmp/serve.
```


# index.html
```
<!doctype html>
<html ng-app="gga">
  <head>
    <meta charset="utf-8">
    <title>gga</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width">
    <!-- Place favicon.ico and apple-touch-icon.png in the root directory -->
 
    <!-- build:css({.tmp/serve,src}) styles/vendor.css -->
    <!-- bower:css -->
    <!-- run `gulp inject` to automatically populate bower styles dependencies -->
    <!-- endbower -->
    <!-- endbuild -->
 
    <!-- build:css({.tmp/serve,src}) styles/app.css -->
    <!-- inject:css -->
    <!-- css files will be automatically insert here -->
    <!-- endinject -->
    <!-- endbuild -->
  </head>
  <body>
    <!--[if lt IE 10]>
      <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
    <![endif]-->
 
    <div ui-view></div>
 
    <!-- build:js(src) scripts/vendor.js -->
    <!-- bower:js -->
    <!-- run `gulp inject` to automatically populate bower script dependencies -->
    <!-- endbower -->
    <!-- endbuild -->
 
    <!-- build:js({.tmp/serve,.tmp/partials,src}) scripts/app.js -->
    <!-- inject:js -->
    <!-- js files will be automatically insert here -->
    <!-- endinject -->
 
    <!-- inject:partials -->
    <!-- angular templates will be automatically converted in js and inserted here -->
    <!-- endinject -->
    <!-- endbuild -->
 
  </body>
</html>
```


# .tmp/serve /index.html

```
<!doctype html>
<html ng-app="gga">
  <head>
    <meta charset="utf-8">
    <title>gga</title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width">
    <!-- Place favicon.ico and apple-touch-icon.png in the root directory -->
 
    <!-- build:css({.tmp/serve,src}) styles/vendor.css -->
    <!-- bower:css -->
    <link rel="stylesheet" href="../bower_components/angular-toastr/dist/angular-toastr.css" />
    <link rel="stylesheet" href="../bower_components/animate.css/animate.css" />
    <!-- endbower -->
    <!-- endbuild -->
 
    <!-- build:css({.tmp/serve,src}) styles/app.css -->
    <!-- inject:css -->
    <link rel="stylesheet" href="app/index.css">
    <!-- endinject -->
    <!-- endbuild -->
  </head>
  <body>
    <!--[if lt IE 10]>
      <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
    <![endif]-->
 
    <div ui-view></div>
 
    <!-- build:js(src) scripts/vendor.js -->
    <!-- bower:js -->
    <script src="../bower_components/jquery/dist/jquery.js"></script>
    <script src="../bower_components/angular/angular.js"></script>
    <script src="../bower_components/angular-animate/angular-animate.js"></script>
    <script src="../bower_components/angular-cookies/angular-cookies.js"></script>
    <script src="../bower_components/angular-touch/angular-touch.js"></script>
    <script src="../bower_components/angular-sanitize/angular-sanitize.js"></script>
    <script src="../bower_components/angular-messages/angular-messages.js"></script>
    <script src="../bower_components/angular-aria/angular-aria.js"></script>
    <script src="../bower_components/angular-resource/angular-resource.js"></script>
    <script src="../bower_components/angular-ui-router/release/angular-ui-router.js"></script>
    <script src="../bower_components/angular-bootstrap/ui-bootstrap-tpls.js"></script>
    <script src="../bower_components/malarkey/dist/malarkey.min.js"></script>
    <script src="../bower_components/angular-toastr/dist/angular-toastr.tpls.js"></script>
    <script src="../bower_components/moment/moment.js"></script>
    <!-- endbower -->
    <!-- endbuild -->
 
    <!-- build:js({.tmp/serve,.tmp/partials,src}) scripts/app.js -->
    <!-- inject:js -->
    <script src="app/index.module3.js"></script>
    <script src="app/components/webDevTec/webDevTec.service.js"></script>
    <script src="app/components/navbar/navbar.directive.js"></script>
    <script src="app/components/malarkey/malarkey.directive.js"></script>
    <script src="app/components/githubContributor/githubContributor.service.js"></script>
    <script src="app/main/main.controller.js"></script>
    <script src="app/index.run.js"></script>
    <script src="app/index.route.js"></script>
    <script src="app/index.module2.js"></script>
    <script src="app/index.constants.js"></script>
    <script src="app/index.config.js"></script>
    <!-- endinject -->
 
    <!-- inject:partials -->
    <!-- angular templates will be automatically converted in js and inserted here -->
    <!-- endinject -->
    <!-- endbuild -->
 
  </body>
</html>
```


# 经过测试发现,js注入在`gulp serve`后生成到 .tmp/serve下(所有工程内js会动态注入,无需手动引入)
对于加载顺序要求,顺序:
1. angular .module 如:  app/index.module3.js [此经过尝试发现,注入(inject)会扫码文件内容,动态决定其引入优先级]
2.app/components/*
3.自定义js 
4. angular.run route constants config 如:
```
    <script src="app/index.run.js"></script>
    <script src="app/index.route.js"></script>
    <script src="app/index.constants.js"></script>
    <script src="app/index.config.js"></script>
```
