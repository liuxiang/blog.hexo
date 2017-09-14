title: 为配合“Android6.0权限检查机制改变”提供的cordova权限插件
date: 2016-07-29 00:00:00
tags: [ Android6.0, 权限检查机制 ]


---


**由于Android6.0权限检查机制的改变.在以前，权限在用户选择安装是授权。现在，需要在使用的过程中显示授权。
对于用到旧的cordova插件可能不支持或者停止更新。所以，需要独立的权限控制插件来补偿。 **


---


# 使用新的插件`cordova-plugin-android-permission`
https://github.com/NeoLSN/cordova-plugin-android-permission


- 官网替代插件已出:`cordova-plugin-compat`(无API调用文档)

https://github.com/apache/cordova-plugin-compat


# 安装

```
cordova plugin add cordova-plugin-android-permissions@0.9.1

```
# 使用示例
```
var permissions = cordova.plugins.permissions;
    permissions.hasPermission(permissions.WRITE_EXTERNAL_STORAGE , checkPermissionCallback, null);
 
    function checkPermissionCallback(status) {
      if(!status.hasPermission) {
        var errorCallback = function() {
          console.warn('Camera permission is not turned on');
        }
 
        permissions.requestPermission(
          permissions.WRITE_EXTERNAL_STORAGE,
          function(status) {
            if(!status.hasPermission) errorCallback();
          },
          errorCallback);
      }
    }
```
# 更多150余种权限参数,请见
https://github.com/NeoLSN/cordova-plugin-android-permission/blob/master/www/permissions.js
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/33817408.jpg)

 


# 效果(测试解决华为android6.0手机，获取存储权限。成功)


![](http://7xnbs3.com1.z0.glb.clouddn.com/16-7-30/33998655.jpg)

![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-7-30/95736569.jpg)
 
   <!-- more -->
