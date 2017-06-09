title: css tips
date: 2016-04-07 00:00:00
categories: css tips
tags: [ css tips ]


---


# 文字前加两个空格

`text-indent: 2em;`



# 字体加粗
`font-weight:bold;`



# 文字多出省略( 设置文本缩略的样式为"...")
```
white-space: nowrap;  /*文本不会换行 */
text-overflow: ellipsis;  /*文本溢出省略*/
overflow: hidden;   /*溢出策略：隐藏*/

```
 word-wrap:break-word;/* 换行 */



` white-space  ` 如何处理元素中空白符
` text-overflow ` 当文本溢出时是否显示


# 多行文字多出省略( 设置文本缩略的样式为"...")

```
p{
  width:200px;
  height: 32px;
  overflow: hidden;
  text-overflow: ellipsis;


  display: -webkit-box;
  -webkit-line-clamp:2;
  -webkit-box-orient: vertical;
  font-size: 14px
}
```
-webkit-line-clamp 用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的WebKit属性
- 1，display: -webkit-box; 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
- 2，-webkit-box-orient 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。
- 3，text-overflow: ellipsis; ，可以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本 。


# 文字阴影


普通文字


`单层`  `text-shadow: 5px -5px 0 rgba(17, 17, 17, 0.45);` 


`双层`  `text-shadow: 5px -5px 0 rgba(17, 17, 17, 0.45), 7px -7px 10px rgba(17, 17, 17, 0.65);`


**参考** http://www.w3cplus.com/blog/52.html


# css模糊效果，毛玻璃，滤镜
```
-webkit-filter: blur(10px); /* Chrome, Opera */
       -moz-filter: blur(10px);
        -ms-filter: blur(10px);   
            filter: blur(10px);
```


http://www.zhangxinxu.com/study/201311/css-filter-image-blur.html
http://www.zhangxinxu.com/wordpress/2015/02/image-local-blur-background-attachment-fixed/
http://www.html5tricks.com/css3-image-fuzzy.html


# `border` 边框控制


```
    border-top: 10px solid #333;
    border-left: 31px solid #f8f8f8;
    border-bottom: 10px solid #333;
    border-right: 10px solid;
```


# `背景渐变`
`background: -webkit-linear-gradient(right,#f8f8f8,#886aea);`


# 分割线
```
.demo_line_01{
    /* padding: 0 20px 0; */
    margin: 20px auto;
    line-height: 1px;
    border-left: 10vw solid #ddd;
    border-right: 10vw solid #ddd;
    text-align: center;
}
```

`CSS巧妙实现分隔线的几种方法_大前端`
http://www.daqianduan.com/4258.html


# css 选择器
`:nth-child(n)` 获取到由变量名定义的个数个子标签
`:eq(n)`  匹配基于文档顺序、序号从0开始的选中列表中的第n个元素(jQuery的扩展)
`:nth(n)` 与“:eq(n)”相同(jQuery的扩展)


`这 30 类 CSS 选择器，你必须记在脑袋里！ - 开源中国社区`
http://www.oschina.net/news/57107/30-css-selector-you-should-remeber



`jQuery选择器和选取方法 - MaxIE - 博客园`
http://www.cnblogs.com/MaxIE/p/4078869.html


# width设成100%background-size不起作用？

```
background-size: 100% 100%;

background: url('../img/...jpg');
```


你这个的问题不是background-size在width:100%;的时候不起作用，而是根本就不起作用，你把background-size放在background属性之后才可以，要不然会被覆盖。
```

background: url('../img/...jpg');

background-size: 100% 100%;

```
https://segmentfault.com/q/1010000004195716/a-1020000004198491


# 高度等于宽度
- css
```
width: 50%;
height: 0;
padding-bottom: 50%;


background-color: #008b57;
```
或
```
width: 50%;

height: calc(width);

```
- js
```
$('#box1').css('height',$('#box1').css('width'));

```
`css中如何规定某一元素高度等于其宽度`
https://segmentfault.com/q/1010000002629233


`请问，如何通过CSS实现高度height随宽度width变化而变化，保持长宽比例不变，宽度是根据父元素宽度变化的？ - hiYoHoo的回答 - SegmentFault`
https://segmentfault.com/q/1010000002790621/a-1020000002791154


`移动端布局，div按比例布局，宽度为百分比，但又想让高度和宽度一样，即让div为正方形，怎么做布局呢？ - 知乎`
https://www.zhihu.com/question/31753528


---


`CSS 参考手册 | 菜鸟教程`
http://www.runoob.com/cssref/css-reference.html


`CSS3参考手册 - CSS3参考手册`
http://www.css88.com/book/css/


<!-- more -->
