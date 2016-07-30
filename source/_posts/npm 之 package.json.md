title: npm 之 package.json
date: 2016-1-7 00:00:02 #发表日期，一般不改动
categories: npm  #文章文类
tags: [npm ,package.json ]


---
npm 之 package.json.md


# `package.json` 帮助查看
`npm help json`


# `npm init` 新建(/更新) `package.json`
> 如果`package.json`文件依据存在不会改变未设置的配置项


```
F:\work\wosai\code.wosai\edu.k12\k12student>npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.


See `npm help json` for definitive documentation on these fields
and exactly what they do.


Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.


Press ^C at any time to quit.
name: (tabs)


version: (1.0.0)
keywords:


author:
license: (ISC)
About to write to F:\work\wosai\code.wosai\edu.k12\k12student\package.json:


{
  "name": "tabs",
  "version": "1.0.0",
  "description": "tabs: An Ionic project",
  "cordovaPlugins": [
    "cordova-plugin-device",
    "cordova-plugin-console",
    "cordova-plugin-whitelist",
    "cordova-plugin-splashscreen",
    "com.ionic.keyboard",
    {
      "locator": "https://github.com/mrwutong/cordova-qdc-alipay.git",
      "id": "com.qdc.plugins.alipay"
    }
  ],
  "cordovaPlatforms": [
    "android"
  ],
  "main": "gulpfile.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://git.oschina.net/xwill/k12student.git"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "gulp": "^3.9.0",
    "gulp-imagemin": "^2.4.0",
    "gulp-concat": "^2.6.0",
    "gulp-jshint": "^1.11.2",
    "gulp-minify-css": "^0.3.13",
    "gulp-minify-html": "^1.0.4",
    "gulp-rename": "^1.2.2",
    "gulp-sass": "^1.3.3",
    "gulp-uglify": "^1.4.2",
    "gulp-util": "^3.0.7",
    "gulp-watch": "^4.3.5",
    "imagemin-pngquant": "^4.2.0"
  },
  "devDependencies": {}
}




Is this ok? (yes)


```


# 奇淫技巧
> 方式一: 删除`package.json`中的`dependencies`项,再`npm init`会依据`node_modules`的实际安装情况,更新` dependencies `配置项.但会多余配置部分信息,可能与原文件不一致,会出现覆盖.


> 方式二: 为减少误操作,可先备份(或重命名)`package.json`,再 `npm init`同样会 依据`node_modules`的实际安装情况,更新` dependencies `配置项


> 如何把node_module所有模块的版本信息更新到package.json中？
重新执行npm init




# 未能解决的问题
*  依据`node_modules`的实际安装情况恢复配置项时无法信息判断恢复位置[ dependencies,devD ependencies ]


<!-- more -->
