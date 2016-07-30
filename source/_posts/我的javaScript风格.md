title: 我的javaScript风格
date: 2016-3-3 00:00:04
categories: javaScript
tags: [javaScript,javaScript风格]
 
---
 
# IIFE(自执行函数)应用 & 严格模式
* `js模板`避免冲突
```
/**
* Created by liuxiang@wosai on 2016/3/3.
*/
(function () {
    'use strict';
 
})();
```


# WebStorm  设置文件模板 `搜-template`->Editor-`File and code Templates`
 


# 构造方法,风格编码
* 优点-引导程序员,更有条理的编写代码[干净,易读,好维护]
```
/**
* Created by liuxiang@wosai on 2016/3/3.
*/
(function () {
  'use strict';
 
  // 此部分为js入口`构造函数`
  {
    /** 介绍:
     * 使用{},未闭合作用域,方便具体方法与js内的全局变量交互.
     * 使用(function(){}())也可作为构造执行,但封闭了与外部交互的通道
     * 优点-引导程序员,更有条理的编写代码[干净,易读,好维护]
     **/
 
    var variable;// 声明变量
    xxx.click(xxxClick);// 行为
    $scope.methodName = methodName;// angular空间行为绑定
 
    init();// 初始化
  }
 
  /**
   * 注意:
   * 构造区{},以外只存在function,不存在立即执行的代码.
   * 避免意想不到的执行结果.
   **/
 
  function init(){
    // 具体执行
  }
 
  function  xxxClick(){
    // 具体执行
  }
 
  function methodName(){
    // 具体执行
  }
 
})();
```
 
<!-- more -->