title: 为Chrome应用程序(postman为例)载入userScript
date: 2016-2-24 00:00:00 #发表日期，一般不改动
categories:  Chrome   #文章文类

tags: [ Chrome,Chrome Dev Tools ]


---

![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-3-14/87650243.jpg)


# Chrome应用程序 `postman`
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-24/24974723.jpg)


问题:
Chrome应用程序 `postman`在我的笔记本分辨率1366*768 下`界面右侧内容溢出`



# 解决思路
使用UserScript进行额外的样式调整,收缩左侧栏.
编写脚本 ` postman.user.js `
```
// ==UserScript==
// @name        postman 1366*768 下界面溢出优化
// @author      liuxiang@wosai
// @description postman 1366*768 下界面溢出优化
// @license     GPL version 3
// @encoding    utf-8
// @date        24/02/2016
// @modified    24/02/2016
// @include     chrome-extension://fhbjgbiflinjbdggehcddcbncdddomop/*
// @include     chrome-extension://fhbjgbiflinjbdggehcddcbncdddomop/requester.html
// @grant       GM_xmlhttpRequest
// @grant       GM_getResourceURL
// @run-at      document-end
// @version     1.0
// ==/UserScript==


// 解决1366*768分辨率下内容溢出. 将其拽回
$("#outer-sidebar").css("width","325px");
$("#main-container").css("margin-left","325px");
```


# 结果
经调试发现,userScript不能对`应用程序`进行监听,执行.


# 新解决思路
将 UserScript脚本放置本地的应用程序中,编辑入口页面,载入用户脚本 ` postman.user.js `


* 查找`postman`本地路径及入口文件
右键属性: chrome-extension://fhbjgbiflinjbdggehcddcbncdddomop/requester.html
可通过搜索找到本地具体位置


* 将 ` postman.user.js `放置与` requester.html `同级,再更新 ` requester.html `末尾引入用户脚本` postman.user.js `
```
// ==UserScript==
// @name        postman 1366*768 下界面溢出优化
// @author      liuxiang@wosai
// @description postman 1366*768 下界面溢出优化
// @license     GPL version 3
// @encoding    utf-8
// @date        24/02/2016
// @modified    24/02/2016
// @include     chrome-extension://fhbjgbiflinjbdggehcddcbncdddomop/*
// @include     chrome-extension://fhbjgbiflinjbdggehcddcbncdddomop/requester.html
// @grant       GM_xmlhttpRequest
// @grant       GM_getResourceURL
// @run-at      document-end
// @version     1.0
// ==/UserScript==


// 解决1366*768分辨率下内容溢出. 将其拽回
$("#outer-sidebar").css("width","325px");
$("#main-container").css("margin-left","325px");


/*
1. 将文件'postman.user.js'拷贝至 postman 目录下
Chrome post App绝对路径:
C:\Users\Administrator\AppData\Local\360Chrome\Chrome\User Data\Default\Extensions\fhbjgbiflinjbdggehcddcbncdddomop\3.2.14_0


2.载入requester.html末尾
<script type="text/javascript" src="postman.user.js"></script>
*/
```


# 效果
问题解决,`界面右侧内容溢出`已经完全可见

![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-24/95097223.jpg)




# 奇淫技巧: 调试应用程序
Chrome窗口化加载的应用程序(如`postman`),按F12是不能显示`Chrome Dev Tools`的.


技巧: 
访问`chrome://inspect/#apps`是可以查看到已经打开的应用程序.点击`inspect`可打开chrome调试器
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/49600881.jpg)




<!-- more -->


