title: 前端css居中文集
date: 2016-3-5 00:00:00
categories:   css居中


tags: [ 前端 , css ]


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



## 常见
`text-align:center;`

` margin:0 auto; `   


## position: relative,  absolute  

```
.divCenter {
  left: 50%;
 
  -webkit-transform: translate(-50%);
  -moz-transform: translate(-50%);
  -ms-transform: translate(-50%);
  -o-transform: translate(-50%);
  transform: translate(-50%);
  transform: translate(-50%);
}
```


##  float
```
...
```


## display:flex
```
...
```


# 垂直居中
单行( line-height与heigth相等,配合center )
```
height :  100px ;
line-height :  100px;

text-align :  center ;

```
多行(vertical-align:middle)
```
display:table-cell;
vertical-align:middle;

```


---
**参考**
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


> `CSS3 display:flex和display:box有什么区别？ - 知乎`
https://www.zhihu.com/question/22991944


#工具 
`居中css How to Center in CSS`
http://howtocenterincss.com/


<!-- more -->
