title: 前端css居中文集
date: 2016-3-5 00:00:00
categories:   css居中


tags: [ 前端 , css ]


---
`CSS居中完整版` http://www.tuicool.com/articles/v6f6byE  =>  https://segmentfault.com/a/1190000004851345

`Erichain/css-center-complete`  https://github.com/Erichain/css-center-complete



## Contents
为了大家更好的理解，我们使用了树形的目录结构。


- [水平方向的居中](https://github.com/Erichain/css-center-complete/tree/master/horizontal)
  + [水平居中行内元素(比如链接或者文本)](https://github.com/Erichain/css-center-complete/tree/master/horizontal/center-inline-element)
  + [水平居中块级元素](https://github.com/Erichain/css-center-complete/tree/master/horizontal/center-block-element)
  + [水平居中多个块级元素](https://github.com/Erichain/css-center-complete/tree/master/horizontal/center-multiple-block-elements)
- [垂直方向的居中](https://github.com/Erichain/css-center-complete/tree/master/vertical)
  + [垂直居中行内元素(比如链接或者文本)](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-inline-element)
    - [单行](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-inline-element/single-line)
    - [多行](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-inline-element/multi-lines)
  + [垂直居中块级元素](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-block-element)
    - [知道元素的高度](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-block-element/know-height)
    - [不知道元素的高度](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-block-element/unknown%20height)
    - [使用flexbox](https://github.com/Erichain/css-center-complete/tree/master/vertical/center-block-element/flexbox)
- [水平垂直都居中](https://github.com/Erichain/css-center-complete/tree/master/horizontal%26%26vertical)
  + [元素的宽高固定](https://github.com/Erichain/css-center-complete/tree/master/horizontal%26%26vertical/fixed-height-and-width)
  + [元素的宽高不固定](https://github.com/Erichain/css-center-complete/tree/master/horizontal%26%26vertical/unknown-height-and-width)
  + [使用flexbox](https://github.com/Erichain/css-center-complete/tree/master/horizontal%26%26vertical/flexbox)


每一种情况都有相应的code pen，可以点击链接查看。


---
# 环境
* `position`
static ：默认值，无特殊定位，对象遵循正常文档流。top，right，bottom，left等属性不会被应用，默认值。
relative：相对定位，对象遵循正常文档流，但将依据top，right，bottom，left等属性在正常文档流中偏移位置。
absolute：绝对定位，对象脱离正常文档流，使用top，right，bottom，left等属性进行绝对定位。而其层叠通过z-index属性定义。
fixed：固定定位，对象脱离正常文档流，使用top，right，bottom，left等属性以窗口为参考点进行定位，当出现滚动条时，对象不会随着滚动。


* `float`
float属性取值：none(默认)、left、right、inherit。


* `display`
display属性取值：none、inline、inline-block、block、table相关属性值、inherit。
display :   inline和block，又叫行内元素和块级元素
display: inline-block; 行内块元素

table相关的属性值可以用来垂直居中

display:flex 多栏多列布局



# 水平居中
`text-align:center;` 行内，行内块级

` margin:0 auto; `     块级元素，居中盒子与父级盒子宽度指定切小于父级宽度,常搭配`width:80%`


*   水平居中-未知宽度

```
.divCenter {
  left: 50%;
 
  -webkit-transform: translate(-50%);
  -moz-transform: translate(-50%);
  -ms-transform: translate(-50%);
  -o-transform: translate(-50%);
  transform: translate(-50%);
}
```


# 垂直居中
## 行内元素或者行内块级元素？(inline 或者 inline-block)

-  单行(line-height与heigth相等,配合center)
`pading-top:10px; pading-bottom:10px`
```
height: 100px;
line-height: 100px;
white-space: nowrap;
```
- 多行 垂直 居中- flex Box 垂直居中


方法一：   display: `table-cell` && ` vertical-align: middle`;
```
.parent {
  display: table;
  width: 200px;
  height: 400px;
}
.child {
  display: table-cell;
  vertical-align: middle;
}
```
方法二
```
.flex-center-vertically {

    display: flex;
    justify-content: center;
    flex-direction: column;  /* 决定主轴的方向（即项目的排列方向） */
    height: 400px;
}
```
-  inline-block 垂直 居中(` ::before` && ` inline-block` && ` vertical-align : middle ` ) - 单行
```
.ghost-center {
    position: relative;
}
.ghost-center::before {
    content: " ";
    display: inline-block;
    height: 100%;
    width: 1%;
    vertical-align: middle;
}
.ghost-center p {
    display: inline-block;
    vertical-align: middle;
}
```


## 块级元素

*   已知高度-垂直居中
```
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    height: 100px;
    margin-top: -50px;
    /* 如果没有使用border-box的话就只需要关心padding和border了 */
}
```
*   未知高度-垂直居中
```
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
}
```


*   flex Box 垂直居中
```
.parent {
    display: flex;
    flex-direction: column; /* 决定主轴的方向（即项目的排列方向） */
    justify-content: center;
}
align-items: center;         /* -垂直居中*/

```


## 横竖都居中
-  元素固定宽高

```
.parent {
    position: relative;
}
 
.child {
    width: 300px;
    height: 100px;
    padding: 20px;
 
    position: absolute;
    top: 50%;
    left: 50%;
 
    margin: -70px 0 0 -170px;
}
```
-  transform控制方法
```
.parent {
    position: relative;
}
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```
- 父子容器配合

```
.parent {
     margin: auto; /* 解决垂直居中，水平居中并不能生效 */
    display: flex;
}
 
.child {
    margin: auto;/* 依赖父级的flex布局，可以使用margin：auto；解决水平居中 */
}
```
### `margin: auto;`生效条件 
- 文档声明: DOCTYPE声明

- 生效场景`(自元素宽度小于父级元素宽度时auto可生效)`
    -  当前标签要求: 自元素需要有宽度(或说自身宽度小于父级宽度,可生效Auto)  

     - 父级 display: flex;  自元素可使用auto
- 失效场景
    - 自身无宽度或宽度与父级一致再或超过父级

    - 浮动了
-  `margin: 0 auto;`与` text-align:center; `区别
    -  `margin: 0 auto;` 通过外边距使自身居中显示

    -  ` text-align:center; ` 通知子集依据自身的位置及宽度, 居中显示



## 全屏方式
- 兼容性最好
```
    position: absolute;

    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
```
- 依据可视宽高(简单)
```
    position: absolute;

    height: 100vh;
    width: 100vw;
```
- 受父级容器影响
```
    position: absolute;

     width: 100%;
    height: 100%;
```


##  全屏方式 , flex Box 横竖都居中
- 不遮盖背景(非全屏,相对全屏横竖居中)
```
    /*  容器位置起点(右上角)居中  */
    position: absolute;
    top: 50%;
    left: 50%;
    

    /* 内容横竖回退50%,使其内容同时居中 */

    transform: translate(-50%, -50%);
```  

  
-  铺满全屏
```
.parent {

    /* 使容器铺满全屏 */
    position: absolute;
    height: 100vh;
    width: 100vw;
 
    /* 设置弹性布局,子集横竖居中 */
    display: flex;                      /*父级flex Box布局*/
    justify-content: center;    /* -水平居中*/
    align-items: center;         /* -垂直居中*/
}


/* 子集处于横竖居中位置 */
```
**父级(全屏) | 子集(不 遮盖父级 ,居中) **

   



或(父子容器配合)

```
.parent {

    position: absolute;
    height: 100vh;
    width: 100vw;
    
    display: flex;         \*父级flex Box布局*\

}


.child {
     margin: auto;     \* 内容可设置margin:auto *\
}
```
** 父级(全屏) | 子集(遮盖父级,用外边距使子集相对居中)**

 

- 分析
`justify-content: center`会改变`.  parent `自身面板位置.
如希望子集其它元素不受影响，可以在子集元素中配合`margin:auto` 使用.  或使用` display: -webkit-box `布局：
```
    display: -webkit-box;
    -webkit-box-pack: center;    /* -水平居中: 其中一半空间被置于首个子元素前，另一半被置于最后一个子元素后 */
    -webkit-box-align: center;    /* -垂直居中: 其中 一半位于子元素之上，另一半位于子元素之下 */
```
- 定义
    -  box-pack 属性规定当框大于子元素的尺寸，在何处放置子元素。

    - box-align 属性规定如何对齐框的子元素 。


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-4-24/75684487.jpg)
<!-- 
    -->
 

# ===========================================> 更多,更强


使用`display: block;` `display: inline-block;` 来改变元素类似，辅助定位



* table-cell 居中
```
.parent {
  display: table;
  width: 200px;
  height: 400px;
}
.child {
     vertical-align: middle;    /*垂直居中*/
     display: table-cell;    /*表格化*/
     word-break: break-all;    /*文字长度达到盒子的宽度后强制换行*/
}


```
` ★ `  `CSS居中完全指南`  http://www.tuicool.com/articles/f6BJn23

`div表格定高垂直居中，水平居中 - 推酷`  http://www.tuicool.com/articles/VjARZbv


*   webkit  box
```
    display: -webkit-box;
    -webkit-box-pack: center;
    -webkit-box-align: center;
    display: -moz-box;
    -moz-box-pack: center;
    -moz-box-align: center;   
    display: -ms-flexbox;
    -ms-flex-pack: center;
    -ms-flex-align: center;
```
`一问就蒙(1)--垂直居中` http://www.tuicool.com/articles/Qj67zqJ


*   -webkit-box &  flex二合一

```
.Center-Container.is-Flexbox {
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-box-align: center;
     -moz-box-align: center;
     -ms-flex-align: center;
  -webkit-align-items: center;
          align-items: center;
  -webkit-box-pack: center;
     -moz-box-pack: center;
     -ms-flex-pack: center;
  -webkit-justify-content: center;
          justify-content: center;
}
```
` ★ ` `如何只用CSS做到完全居中`
http://blog.jobbole.com/46574/
* 原文  http://codepen.io/shshaw/full/gEiDt
* 专题  `Designing CSS Layouts With Flexbox Is As Easy As Pie – Smashing Magazine` 
https://www.smashingmagazine.com/2013/05/centering-elements-with-flexbox/


---
**更多参考**
`讲一讲CSS的position/float/display都有哪些取值，它们相互叠加时的行为都是什么？_许乐乐_新浪博客`
http://blog.sina.com.cn/s/blog_5e4b495d0101738w.html


` ★ `  `CSS居中完全指南`
http://www.tuicool.com/articles/f6BJn23


` ★ ` `如何只用CSS做到完全居中`
http://blog.jobbole.com/46574/


`盘点8种CSS实现垂直居中水平居中的绝对定位居中技术`  源自=>  `如何只用CSS做到完全居中`

http://blog.csdn.net/freshlover/article/details/11579669


`从一个居中方法说起 —— 谈 translate 与 相对、绝对定位 - SegmentFault`
http://segmentfault.com/a/1190000002436755


`CSS实现元素水平/垂直居中的方法 - xiuhong的个人页面 - 开源中国社区`

http://my.oschina.net/xiuhong/blog/261346


 `CSS水平居中/垂直居中的N个方法 前端开发必收藏 - CSS - 酷站代码`
http://www.5icool.org/a/201309/a2516.html


`CSS实现居中的7种方法 - chenmoquan的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/chenmoquan/article/details/41547609


** display:flex **
> `Flex 布局教程：语法篇 - 阮一峰的网络日志`
http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html


>`Flex 布局教程：实例篇 - 阮一峰的网络日志`
http://www.ruanyifeng.com/blog/2015/07/flex-examples.html?bsh_bid=683103006



> `CSS3 display:flex和display:box有什么区别？ - 知乎`
https://www.zhihu.com/question/22991944


` ★ `  `使用 CSS 弹性盒 - CSS | MDN`
https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Using_CSS_flexible_boxes
![]( https://mdn.mozillademos.org/files/12998/flexbox.png)
<!--
-->



`结合 CSS3 的布局新特征谈谈常见布局方法`
http://www.tuicool.com/articles/QrUZzyu


`这可能是史上最全的CSS自适应布局总结`
http://www.tuicool.com/articles/rYjyiyz


`CSS布局自适应等分比例实践-前端开发博客`
http://caibaojian.com/css-equal-layout.html


# 工具 
` 居中场景选择css  How to Center in CSS`
http://howtocenterincss.com/
https://github.com/oliverzheng/howtocenterincss


` 居中场景选择 css   Centering in CSS: A Complete Guide | CSS-Tricks`
https://css-tricks.com/centering-css-complete-guide/
https://github.com/Erichain/css-center-complete



---
# 图片与文字垂直居中

```
img{
     vertical-align: middle;

}
```
`文字与图片在同一行垂直居中问题 `
http://blog.csdn.net/tool163/article/details/7088236


<!-- more -->
