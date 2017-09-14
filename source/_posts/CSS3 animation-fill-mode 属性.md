title: CSS3 animation-fill-mode 属性
date: 2016-08-26 00:00:00
tags: [css]


---
`CSS3 animation-fill-mode 属性`
http://www.w3school.com.cn/cssref/pr_animation-fill-mode.asp
 
# 定义和用法
animation-fill-mode 属性规定动画在播放之前或之后，其动画效果是否可见。
- 注释：其属性值是由逗号分隔的一个或多个填充模式关键词。

-   默认值：    none
-   继承性：    no
-   版本：    CSS3
-   JavaScript 语法：    object.style.animationFillMode=none


# 语法
animation-fill-mode : none | forwards | backwards | both;
值    描述
none    不改变默认行为。
forwards    当动画完成后，保持最后一个属性值（在最后一个关键帧中定义）。
backwards    在 animation-delay 所指定的一段时间内，在动画显示之前，应用开始属性值（在第一个关键帧中定义）。
both    向前和向后填充模式都被应用。


none 表示不设置结束之后的状态，默认情况下回到跟初始状态一样。
forwards 表示将动画元素设置为整个动画结束时的状态。
backwards 明确设置动画结束之后回到初始状态。
both 表示设置为结束或者开始时候的状态。一般都是回到默认状态。


## 快捷语法: 
```
animation: assetAnim 2s 1 forwards;

```


---
#  animation   delay延时与 visibility的结合应用
常见问题: delay延时还没开始前` visibility`就已经显示出来,而将` visibility`放置动画过程中` @keyframes` ,播放后样式又消失
处理办法:结合` animation-fill-mode: forwards `可以保留最后一帧的属性(即` visibility: visible` )


# 实例一:
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/69071103.jpg)



- html
```
<div class="ele"></div>

```
- css
```
<style>
.ele {
  width: 60px;
  height: 60px;
 
  background-color: #ff6699;
  animation: 1s fadeIn;
  animation-fill-mode: forwards;
 
  visibility: hidden;
}
 
.ele:hover {
  background-color: #123;
}
 
@keyframes fadeIn {
  99% {
    visibility: hidden;
  }
  100% {
    visibility: visible;
  }
}
</style>
```
`Pure CSS animation visibility with delay   - Stack Overflow `
http://stackoverflow.com/questions/30855985/pure-css-animation-visibility-with-delay


# 示例二 ： 全css实现动画延时前隐藏，后显示
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/2471608.jpg)
 
```
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div class="stars">
          <div class="star _slideDown _d-1">X</div>
          <div class="star _slideDown _d-2">X</div>
          <div class="star _slideDown _d-3">X</div>
          <div class="star _slideDown _d-4">X</div>
          <div class="star _slideDown _d-5">X</div>
        </div>
</body>
</html>


```


- css
注意: visibility: visible !important; 在`@keyframes`动画中不能被识别
```
<style>
.stars {
      display: flex;
      text-align: center;
      padding: 5vw 0;
}
.star {
    flex: 1;
    font-size: 3.5em;
    color: white;
    //visibility: hidden;// 动画前隐藏
    //opacity: 0;
    background: #9E9E9E;
    border-radius: 50%;
}
 
</style>
 
<style>
._slideDown {
  animation-name: _slideDown;
  -webkit-animation-name: _slideDown;
 
  animation-duration: 1s;
  -webkit-animation-duration: 1s;
 
  animation-timing-function: ease;
  -webkit-animation-timing-function: ease;
 
  /*animation-delay: 2s;*/
  animation-fill-mode: forwards;
  visibility: hidden;
}
 
._slideDown._d-1{
animation-delay: 1s;
}
._slideDown._d-2{
animation-delay: 2s;
}
._slideDown._d-3{
animation-delay: 3s;
}
._slideDown._d-4{
animation-delay: 4s;
}
._slideDown._d-5{
animation-delay: 5s;
}
 
@keyframes _slideDown {
  0% {
    visibility: visible;
    transform: translateY(-100%);
  }
  50% {
    transform: translateY(8%);
  }
  65% {
    transform: translateY(4%);
  }
  80% {
    transform: translateY(3%);
  }
  95% {
    transform: translateY(2%);
  }
  100% {
    visibility: visible;
    transform: translateY(0%);
  }
}
 
@-webkit-keyframes _slideDown {
  0% {
      visibility: visible;
    -webkit-transform: translateY(-100%);
  }
  50% {
    -webkit-transform: translateY(8%);
  }
  65% {
    -webkit-transform: translateY(4%);
  }
  80% {
    -webkit-transform: translateY(3%);
  }
  95% {
    -webkit-transform: translateY(2%);
  }
  100% {
      visibility: visible;
    -webkit-transform: translateY(0%);
  }
}
</style>
```


---


`css3 - image disappears after CSS animation completes - Stack Overflow`
http://stackoverflow.com/questions/31406768/image-disappears-after-css-animation-completes

 