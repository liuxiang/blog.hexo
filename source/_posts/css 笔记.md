title: css 笔记
date: 2015-10-28 00:00:00 #发表日期，一般不改动
categories:  前端  #文章文类
tags: [前端,css] #文章标签，多于一项时用这种格式
photos:


---
div显示边框,便于定位预览
```
.bs{

  div{
    border: solid 1px red;
  }
}
```
单行-垂直居中

```

style =" height :  100px ; text-align :  center ; line-height :  100px "   

思路： line-height与heigth相等
提示：text-align: center 依赖父元素宽度
```


相对/绝对定位, 变形（Transforms）居中
``` css
position: relative;

left: 50%;


-webkit-transform: translate(-50%);
-moz-transform: translate(-50%);
-ms-transform: translate(-50%);
-o-transform: translate(-50%);
transform: translate(-50%);
```
>`从一个居中方法说起 —— 谈 translate 与 相对、绝对定位 - SegmentFault`
http://segmentfault.com/a/1190000002436755


>`盘点8种CSS实现垂直居中水平居中的绝对定位居中技术 - freshlover的专栏 - 博客频道 - CSDN.NET`

http://blog.csdn.net/freshlover/article/details/11579669


>`CSS实现元素水平/垂直居中的方法 - xiuhong的个人页面 - 开源中国社区`

http://my.oschina.net/xiuhong/blog/261346


>`CSS水平居中/垂直居中的N个方法 前端开发必收藏 - CSS - 酷站代码`

http://www.5icool.org/a/201309/a2516.html


<!-- more -->
