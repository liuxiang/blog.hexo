
title: 支付宝六位密码框(alipay sixDigitPassword)
date: 2016-04-25 00:00:00
categories: 六位密码框
tags: [六位密码框]


---


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-5/48657885.jpg)


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-5-5/17867479.jpg)


# github Search
` 支付宝 密码 `
https://github.com/search?l=JavaScript&q=%E6%94%AF%E4%BB%98%E5%AE%9D+%E5%AF%86%E7%A0%81&ref=searchresults&type=Repositories&utf8=%E2%9C%93


`sixDigitPassword`
https://github.com/search?q=sixDigitPassword&ref=searchresults&type=Code&utf8=%E2%9C%93


# 实现逻辑
javaScript控制，界面密码，光标图片代替。


# 简化实现
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-5-5/25214920.jpg)



* 思路：
`letter-spacing`控制字间距
分割线盖在头上（不能有宽度，会阻止文本框获得焦点）
各事件控制，输入格式与长度


* 遗留问题
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-5-5/32584045.jpg)



虽控制了第5位的失去焦点，但在键盘按下，弹起之间还是会出现错位现象。

```
      <div class="codeNum_group">
        <input type="text" class="codeNum"
               onclick="var $Val = $.trim($(this).val());$(this).val('').focus().val($Val)"
               onkeyup="value=value.replace(/[^\d]/g,'');value.length >= 4 ? $(this).blur():''"
               ng-click="focus_ent();"
               ng-model="codeNum" ng-change="blur_4()"
               maxlength="4">
        <div class="division one"></div>
        <div class="division"></div>
        <div class="division"></div>
        <div class="division"></div>
        <div style="clear: both;"></div>
      </div>
```
```
<style>
  .codeNum_group {
    margin-top: 10px;
    display: flex;
    flex-wrap: wrap;
    position: relative;
    width: 160px;
  }
 
  .codeNum {
    letter-spacing: 32px;
    padding-left: 15px !important;
    height: 40px !important;
    width: 160px;
    border-radius: 5px;;
  }
 
  .division.one {
    margin-left: -160px;
    border: 0;
  }
 
  .division {
    height: 40px;
    margin-left: 39px;
    background: red;
    background: rgba(255, 255, 255, 0.15);
    border-right: solid 1px #ddd;
  }
</style>
```


<!-- more -->
