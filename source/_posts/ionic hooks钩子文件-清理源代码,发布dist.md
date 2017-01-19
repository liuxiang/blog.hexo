title: ionic hooks钩子文件-清理源代码,发布dist
date: 2016-10-08 00:00:00
tags: [ ionic ]


---
`020_dist_to_platforms_v2.js` 

- 策略不同,避开 mv_path() 同步控制, 效果一样
```
#!/usr/bin/env node
 
{
  var fs = require('fs');
  var path = require('path');
  var mv = require('mv');
 
  var iosPlatformsDir_dist = path.resolve(__dirname, '../../platforms/ios/www/dist');
  var iosPlatformsDir = path.resolve(__dirname, '../../platforms/ios/www');
 
  var androidPlatformsDir_dist = path.resolve(__dirname, '../../platforms/android/assets/www/dist');
  var androidPlatformsDir = path.resolve(__dirname, '../../platforms/android/assets/www');
 
  /** ios ****************************************************************************/
  action(iosPlatformsDir, iosPlatformsDir_dist)
  /** android ****************************************************************************/
  action(androidPlatformsDir, androidPlatformsDir_dist)
 
}
 
function action(platformsDir, platformsDir_dist) {
  // 1.先删除(因为删除是同步的,移动是异步)
  deleteFolderRecursive(platformsDir + '/app');
  deleteFolderRecursive(platformsDir + '/app_module');
  deleteFolderRecursive(platformsDir + '/autoTest');
  deleteFolderRecursive(platformsDir + '/css');
  deleteFolderRecursive(platformsDir + '/js');
  deleteFolderRecursive(platformsDir + '/templates');
  deleteFolderRecursive(platformsDir_dist + '/img');//  使用源文件资源,不需要dist中资源
  deleteFolderRecursive(platformsDir_dist + '/lib');//  使用源文件资源,不需要dist中资源
 
  // 2.移动dist目录中资源为主资源
  mv_path(platformsDir_dist + '/dist_css', platformsDir + '/dist_css');
  mv_path(platformsDir_dist + '/dist_js', platformsDir + '/dist_js');
  mv_path(platformsDir_dist + '/index.html', platformsDir + '/index.html');
 
  // 3.清理lib(仅保留字体)
  mv_path(platformsDir + '/lib/ionic/fonts', platformsDir + '/_lib/ionic/fonts',function(){
    deleteFolderRecursive(platformsDir + '/lib');
    mv_path(platformsDir + '/_lib', platformsDir + '/lib');// 还原保留
  });
}
 
/** 移动文件 */
function mv_path(source, target, callback) {
  mv(source, target, {mkdirp: true}, function (err) {
    if (typeof err != 'undefined') {
      console.log("err");
      console.log(err);
      console.log("ERROR when moving CSS folder to iOS platform");
    }
    else {
      console.log("CSS folder moved OK to iOS platform");
      callback && callback();
    }
  });
};
 
/** 删除文件 */
function deleteFolderRecursive(removePath) {
  if (fs.existsSync(removePath)) {
    fs.readdirSync(removePath).forEach(function (file, index) {
      var curPath = path.join(removePath, file);
      if (fs.lstatSync(curPath).isDirectory()) { // recurse
        deleteFolderRecursive(curPath);
      } else { // delete file
        fs.unlinkSync(curPath);
      }
    });
    fs.rmdirSync(removePath);
  }
};
```


---


`020_dist_to_platforms_v1.js`
- 脏代码:mv_path() 同步控制

```
#!/usr/bin/env node
 
{
  var fs = require('fs');
  var path = require('path');
  var mv = require('mv');
 
  var iosPlatformsDir_dist = path.resolve(__dirname, '../../platforms/ios/www/dist');
  var iosPlatformsDir_temp = path.resolve(__dirname, '../../platforms/ios/_www_dist');
  var iosPlatformsDir = path.resolve(__dirname, '../../platforms/ios/www');
 
  var androidPlatformsDir_dist = path.resolve(__dirname, '../../platforms/android/assets/www/dist');
  var androidPlatformsDir_temp = path.resolve(__dirname, '../../platforms/android/assets/_www_dist');
  var androidPlatformsDir = path.resolve(__dirname, '../../platforms/android/assets/www');
 
  var cordova_js = '/cordova.js', cordova_plugins = '/cordova_plugins.js';
 
  // 0.转移`cordova.js,cordova_plugins.js`两核心文件
  mv_path(iosPlatformsDir + cordova_js, iosPlatformsDir_dist + cordova_js, function () {
    mv_path(iosPlatformsDir + cordova_plugins, iosPlatformsDir_dist + cordova_plugins, function () {
      mv_path(iosPlatformsDir + '/plugins', iosPlatformsDir_dist + '/plugins', function () {
        mv_path(iosPlatformsDir + '/cordova-js-src', iosPlatformsDir_dist + '/cordova-js-src', function () {
 
          // 1.转移dist目录
          mv_path(iosPlatformsDir_dist, iosPlatformsDir_temp, function () {
            // 2.删除dist外源码
            deleteFolderRecursive(iosPlatformsDir);
            // 3.dist`转正`
            mv_path(iosPlatformsDir_temp, iosPlatformsDir);
          });
 
        })
      })
    });
  });
 
 
  // 0.转移`cordova.js,cordova_plugins.js`两核心文件
  mv_path(iosPlatformsDir + cordova_js, iosPlatformsDir_dist + cordova_js, function () {
    mv_path(iosPlatformsDir + cordova_plugins, iosPlatformsDir_dist + cordova_plugins, function () {
      mv_path(iosPlatformsDir + '/plugins', iosPlatformsDir_dist + '/plugins', function () {
        mv_path(iosPlatformsDir + '/cordova-js-src', iosPlatformsDir_dist + '/cordova-js-src', function () {
 
          mv_path(androidPlatformsDir_dist, androidPlatformsDir_temp, function () {
            deleteFolderRecursive(androidPlatformsDir);
            mv_path(androidPlatformsDir_temp, androidPlatformsDir);
          });
 
        })
      })
    });
  });
 
}
 
/** 移动文件 */
function mv_path(source, target, callback) {
  mv(source, target, {mkdirp: true}, function (err) {
    if (typeof err != 'undefined') {
      console.log("err");
      console.log(err);
      console.log("ERROR when moving CSS folder to iOS platform");
    }
    else {
      console.log("CSS folder moved OK to iOS platform");
      callback && callback();
    }
  });
};
 
/** 删除文件 */
function deleteFolderRecursive(removePath) {
  if (fs.existsSync(removePath)) {
    fs.readdirSync(removePath).forEach(function (file, index) {
      var curPath = path.join(removePath, file);
      if (fs.lstatSync(curPath).isDirectory()) { // recurse
        deleteFolderRecursive(curPath);
      } else { // delete file
        fs.unlinkSync(curPath);
      }
    });
    fs.rmdirSync(removePath);
  }
};
```