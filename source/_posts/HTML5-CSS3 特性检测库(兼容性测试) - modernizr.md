title:  HTML5 /CSS3  特性检测库 ( 兼容性测试 ) - modernizr
date: 2016-08-09 00:00:00
tags: [ HTML5 , 特性检测库, 兼容性测试 ]


---
&emsp; &emsp; Modernizr帮助我们检测浏览器是否实现了某个feature，如果实现了那么开发人员就可以充分利用这个feature做一些工作，反之没有实现开发人员也好提供一个fallback。所以，我们要明白的是Modernizr只是帮我们检测feature是否被支持，它并不能够给浏览器添加那些本来不支持的feature。



# Modernizr有一个 test页面

http://modernizr.github.io/Modernizr/test/index.html
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/63662667.jpg)


&emsp; &emsp; 言归正传。当我们引入了Modernizr.js类库后， <html> 标签的class属性就会被相应的赋值，以显示浏览器是否支持某类CSS属性。比如在IE6下面，不支持boderradius特性，那么在 <html> 标签中就会出现 no-borderradius 类，我们可以做一些fallback的工作：


```
.no-borradius div{
    /*-- do some hacks here --*/
}
```


---
# 使用
- 引入
```
<link rel="stylesheet" href="css/normalize.css">
<script src="js/vendor/modernizr-2.8.3.min.js"></script>
```
- 向<html>元素添加“no-js”的类。
```

<html class="no-js">
```



# 打开页面，F12观察（是否存在`no-`开头的特性标记）
```
<html class=" js flexbox canvas canvastext webgl touch geolocation postmessage websqldatabase indexeddb hashchange history draganddrop websockets rgba hsla multiplebgs backgroundsize borderimage borderradius boxshadow textshadow opacity cssanimations csscolumns cssgradients cssreflections csstransforms csstransforms3d csstransitions fontface generatedcontent video audio localstorage sessionstorage webworkers applicationcache svg inlinesvg smil svgclippaths" lang="">
```


# 依据浏览器支持能力加载资源 `Modernizr.load()`
```
Modernizr.load(
    test: Modernizr.webgl,
    yep : 'three.js',
    nope: 'jebgl.js'
);
```
&emsp; &emsp; 当浏览器支持WebGL的时候，就引入 three.js 这个类库做一些3D效果。浏览器不支持WebGL的时候可以使用 jebgl.js 做一些fallback操作.


&emsp; &emsp; 还有一个比较酷的例子来自官方文档。我们在用jQuery类库的时候，通常都是这种写法：

```

<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js"></script>
<script>window.jQuery || document.write('<script src="js/libs/jquery-1.7.1.min.js">\x3C/script>')</script>
```
现在用Modernizr.load()可以这么写：
```
Modernizr.load([
  {
    load: '//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js',
    complete: function () {
      if ( !window.jQuery ) {
            Modernizr.load('js/libs/jquery-1.7.1.min.js');
      }
    }
  },
  {
    // This will wait for the fallback to load and
    // execute if it needs to.
    load: 'needs-jQuery.js'
  }
]);
```


---
http://html5test.com/
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/90259375.jpg)
 
---




`Modernizr.js入门指南 - 推酷`

http://www.tuicool.com/articles/UVnEVj


`文档 - Modernizr 中文网`
http://modernizr.cn/docs/


`HTML5 Modernizr - HTML5 中文教程 - 极客学院Wiki`
http://wiki.jikexueyuan.com/project/html5/modernizr.html
