title: Safari远程调试(inspect)移动端webview,找不到待调试页面
date: 2016-3-14 00:00:02
categories:   inspect
tags: [ Safari  ,  inspect ]


---
# Safari 远程调试移动端webview, 找不到 待调试 页面



# 原因
 ios设备升级到9.0,默认的开发者权限是关闭的


# 解决
需要设置开发者权限
设置-开发者- instruments -Logging-Energy[打开]
设置-开发者-ui Automation- Enable UI Automation[打开]


<!-- more -->
