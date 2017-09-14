title: Chrome DevTools Devices inspect无法显示调试工具
date: 2016-2-23 00:00:00 #发表日期，一般不改动
categories: inspect  #文章文类
tags: [ Chrome Dev Tools ,   Chrome DevTools,  inspect ]


---



# Chrome DevTools Devices inspect无法显示调试工具(页面空白)
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/24830606.jpg)


## 原因
首次需要登录google服务(即访问站点: appspot.com) [首次访问会缓存状态,二次访问时较快,且不依赖代理]
常规设备未设置代理或host情况下不能正常访问 appspot.com 以致于无法打开页面.


## 解决办法
使用代理工具或配置本地host
我使用的chrome插件"壁虎漫步"
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/55846780.jpg)


``` 
国外地址· 下载 ：
https://chrome.google.com/webstore/detail/%E5%A3%81%E8%99%8E%E6%BC%AB%E6%AD%A5/iahlmnbegagknnkkldncbplimibpcomf?utm_campaign=en&utm_source=en-et-na-us-oc-webstrhm&utm_medium=et


国内地址 · 下载：
http://45.124.67.101:8080/?code=JTCB136VTElC4fEvYQrxt1avWqWdrhdO&index=0
```


# 效果
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/74546907.jpg)


**参考**
http://ask.csdn.net/questions/183926
```
Remote debugging with Chrome downloads files from  https://chrome-devtools-frontend.appspot.com .
Test your connection: Try to open  https://chrome-devtools-frontend.appspot.com
If it doesn't work, thats why you're getting an empty window. In this case maybe you need a VPN.
This is really a problem for people in China because the appspot.com domain is blocked by GFW. So sad.



远程调试与Chrome从https://chrome-devtools-frontend.appspot.com下载文件。
测试您的连接:试着打开https://chrome-devtools-frontend.appspot.com
如果它不工作,这就是为什么你要一个空的窗口。在这种情况下,你可能需要一个VPN。
这是中国人的问题,因为appspot.com域被GFW。很伤心。
```


<!-- more -->
