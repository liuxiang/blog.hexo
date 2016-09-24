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


---


`CSS 参考手册 | 菜鸟教程`
http://www.runoob.com/cssref/css-reference.html


`CSS3参考手册 - CSS3参考手册`
http://www.css88.com/book/css/


<!-- more -->
