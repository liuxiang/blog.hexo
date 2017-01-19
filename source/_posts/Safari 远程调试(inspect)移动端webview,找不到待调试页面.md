title: Safari远程调试(inspect)移动端webview,找不到待调试页面
date: 2016-3-14 00:00:02
categories:   inspect
tags: [ Safari  ,  inspect ]


---
# Safari 远程调试移动端webview, 找不到 待调试 页面



# 原因
 ios设备升级到9.0,默认的开发者权限是关闭的


## 解决
需要设置开发者权限
设置-开发者- instruments -Logging-Energy[打开]
设置-开发者-ui Automation- Enable UI Automation[打开]


## 原因二
- Build Settings 中Provisioning Profile中设置了`xxx_dis`发布版


- 解决办法：修改为`xxx_dev`或者`automatic`,重新安装到程序包，就能正常在Safari中正常调试了


---
# 常规Safari远程调试
```
设置
   | Safari
   |        | 高级
   |        |      |Web检查器
```
`Mac下ionic调试 - java_speed的个人页面 - 开源中国社区`
https://my.oschina.net/twinkling/blog/504824?p={{totalPage}}



---
# 更多调试工具
使用weinre调试

使用chrome的inspect调试

GapDebug

PhoneGap Developer App

Ionic View

`ionic调试方法总结 - ioniconline的博客 - 博客频道 - CSDN.NET`
http://blog.csdn.net/ioniconline/article/details/50162685




<!-- more -->
