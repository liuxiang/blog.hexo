title:  yeoman 之 generator-gulp-angular,引入BootStrap


date: 2017-03-28 00:00:00
tags: [yeoman ]



---

# generator-gulp-angular 中使用`BootStrap`会遇到各种问题
- BootStrap未生效
- BootStrap相关插件不可用（下拉组件）
- `.tmp/serve/index.html`中没有`bootstrap.js`文件的引入


解决办法：
- conf.js （取消bootstrap.js排除）
```
exports.wiredep = {
  exclude: [/\/bootstrap\.js$/, /\/bootstrap-sass\/.*\.js/, /\/bootstrap\.css/],
  directory: 'bower_components'
};
修改为
exports.wiredep = {
  // exclude: [/\/bootstrap\.js$/, /\/bootstrap-sass\/.*\.js/, /\/bootstrap\.css/],
  directory: 'bower_components'
};
```
https://github.com/taptapship/wiredep


- bower.json （引入字体）
```

"overrides": {
  "bootstrap-sass": {
    "main": [
      "assets/stylesheets/_bootstrap.scss",
      "assets/javascripts/bootstrap.js",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.eot",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.svg",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.ttf",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.woff",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.woff2"
    ]
  }
}
```


---
见官方问题汇总：
`新增 bootstrap · Issue #943 · Swiip/generator-gulp-angular`
https://github.com/Swiip/generator-gulp-angular/issues/943



`Bootstrap Dropdown不起作用·问题＃1019·Swiip / generator-gulp-angular`

https://github.com/Swiip/generator-gulp-angular/issues/1019
```
清除exports.wiredep.excludegulp / conf.js以启用jQuery

```


`Bootstrap JS没有映入（not included） ＃882`
https://github.com/Swiip/generator-gulp-angular/issues/882


`Bootstrap下拉菜单不工作 ＃1019`
https://github.com/Swiip/generator-gulp-angular/issues/1019


`bootstrap glyphicon字体路径在构建 ＃903 上断开`
https://github.com/Swiip/generator-gulp-angular/issues/903
```
"overrides": {
  "bootstrap-sass": {
    "main": [
      "assets/stylesheets/_bootstrap.scss",
      "assets/javascripts/bootstrap.js",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.eot",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.svg",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.ttf",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.woff",
      "assets/fonts/bootstrap/glyphicons-halflings-regular.woff2"
    ]
  }
}
```


`为什么设置Jasny Bootstrap，排除 ＃634`
https://github.com/Swiip/generator-gulp-angular/issues/634


