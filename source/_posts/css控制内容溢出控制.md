title: css控制内容溢出控制
date: 2016-3-8 00:00:00
categories:   css


tags: [ 前端 , css ]


---
css控制内容溢出时，使用`...`末尾


# Html
```
<div class=" overflow ">ssssssssssssssssssssssssssssssssssssss<div>
```


# CSS
```
.overflow{
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
```
# 效果
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-3-14/80967330.jpg)
<!-- 
  `=>`    -->



**参考**
` ionic css .item`
https://github.com/driftyco/ionic/blob/master/scss/_items.scss   line:150

```
// Handle text overflow
.item,
.item h1,
.item h2,
.item h3,
.item h4,
.item h5,
.item h6,
.item p,
.item-content,
.item-content h1,
.item-content h2,
.item-content h3,
.item-content h4,
.item-content h5,
.item-content h6,
.item-content p {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
```


# 还原多行
```
.item-text-wrap{
  overflow: visible;

  white-space: normal;
}

```
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-3-14/73237505.jpg)
<!--
  =>   -->


**参考**
https://github.com/driftyco/ionic/blob/master/scss/_items.scss     line:213
```
.item-text-wrap .item,
.item-text-wrap .item-content,
.item-text-wrap,
.item-text-wrap h1,
.item-text-wrap h2,
.item-text-wrap h3,
.item-text-wrap h4,
.item-text-wrap h5,
.item-text-wrap h6,
.item-text-wrap p,
.item-complex.item-text-wrap .item-content,
.item-body h1,
.item-body h2,
.item-body h3,
.item-body h4,
.item-body h5,
.item-body h6,
.item-body p {
  overflow: visible;
  white-space: normal;
}
```


<!-- more -->
