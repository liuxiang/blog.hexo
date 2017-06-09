title: ionic隐藏系统返回键 & 自定义返回键
date: 2016-11-18 00:00:00
tags: [ ionic ]



---
# hide-back-button
```
<ion-view view-title="活动中心" class="appBg page-myMessage" hide-back-button="true">
 
 <ion-nav-buttons side="left">
    <i class="button ion-ios-arrow-left back " style="font-size: 30px;border: 0;" ng-click="navBack()"></i>
  </ion-nav-buttons>
 
  <ion-content>
```
- 更好的样式(Button相比i具备自动居中的能力,整体布局与ionic自有机制保持一致)
```
<ion-view class="feedback" view-title="帮助与反馈" hide-back-button="true">
 
  <ion-nav-buttons side="left">
    <!--<i class="button ion-ios-arrow-left back " style="font-size: 30px;border: 0;" ng-click="navBack()"></i>-->
    <button ng-click="navBack()" class="button buttons button-clear header-item">
      <i class="icon ion-ios-arrow-back"></i>
      <span class="back-text" style="transition-duration: 0ms; transform: translate3d(0px, 0px, 0px);">
          <span class="default-title">&emsp;&emsp;</span>
        </span>
    </button>
 
  </ion-nav-buttons>
 
  <ion-nav-buttons side="right">
    <button class="button button-clear icon" ng-click="feedbackList()">反馈记录</button>
  </ion-nav-buttons>
```


---
# 头部/底部添加按钮
- 头部/底部可以添加按钮，按钮的样式根据头部/底部来设定，所以你不需要为按钮添加额外的样式。
```
<div class="bar bar-header">
  <button class="button icon ion-navicon"></button>
  <h1 class="title">Header Buttons</h1>
  <button class="button">Edit</button>
</div>
```
- button-clear 类可以设置无背景和边框的头部/底部按钮。
```
<div class="bar bar-header">
  <button class="button button-icon icon ion-navicon"></button>
  <div class="h1 title">Header Buttons</div>
  <button class="button button-clear button-positive">Edit</button>
</div>
```
http://www.runoob.com/ionic/ionic-button.html


---
# 解决ios键盘输入焦点时,切换页面导致崩溃问题
```

function navBack() {
  setTimeout(function () {
    $ionicHistory.goBack();
  })
}
```