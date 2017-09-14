title: css实现 ripple 涟漪效果(水纹效果)
date: 2016-08-23 00:00:00
tags: [ css , 涟漪效果 ]


---
# 代码
- home.html
```

<div class="paper">
  <img class="section_img"  ng-src="{{section.pic}}">
  <div class="ripple"></div>
</div>
```


- *.js
```
    /**
     * 涟漪效果 ripple
     */
    function animate_ripple() {
      return true;// 影响性能,暂时关闭
      $(".paper")
        .click(function (e) {
          var ripple = $(this).find(".ripple");
          ripple.removeClass("animate-ripple");
          var x = parseInt(e.pageX - $(this).offset().left) - (ripple.width() / 2);
          var y = parseInt(e.pageY - $(this).offset().top) - (ripple.height() / 2);
          ripple.css({
            top: y,
            left: x
          }).addClass("animate-ripple");
        })
    }
```


-  ripple.scss
```
@charset "utf-8";
 
/** ripple 涟漪效果 begin ***************************/
.animate-ripple {
  -webkit-animation: ripple 0.3s linear;
  animation: ripple 0.3s linear;
}
@-webkit-keyframes ripple {
  100% {
    -webkit-transform: scale(12);
    transform: scale(12);
    background-color: transparent;
  }
}
@keyframes ripple {
  100% {
    -webkit-transform: scale(12);
    transform: scale(12);
    background-color: transparent;
  }
}
 
.paper{
  position: relative;
  width: 30vw;
  height: 30vw;
  margin: auto;
  overflow: hidden;
  -webkit-border-radius:50%;
  -moz-border-radius:50%;
  border-radius:50%;
  -webkit-mask-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAIAAACQd1PeAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAA5JREFUeNpiYGBgAAgwAAAEAAGbA+oJAAAAAElFTkSuQmCC);
}
 
.ripple{
  background-color: rgba(0, 0, 0, 0.45);
  -webkit-border-radius:100%;
  -moz-border-radius:100%;
  border-radius: 100%;
  height: 5vw;
  width: 5vw;
  //margin-top: -30vw;
  position: absolute;
  -webkit-transform: scale(0);
  transform: scale(0);
  //left: 50px;
}
/** ripple 涟漪效果 end ***************************/
```


`css3点击涟漪效果 - 白小虫 - 博客园`

http://www.cnblogs.com/baixc/p/5046922.html



---
# Button的涟漪效果(水纹效果)
`Android L Ripple Click Effect Demos`

http://www.cssscript.com/android-l-ripple-click-effect-with-javascript-and-css3/


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/3481297.jpg)


`涟漪效果按钮的实现 - 笔记 - 前端网（W3Cfuns）`
http://www.w3cfuns.com/notes/12348/0522857f167c46a937e6962c0007259e.html

 


---
# 解决 border-radius 元素在应用了 transform 的子元素 时overflow:hidden 失效的问题

解决方案：
使用webkit-mask-image 覆盖圆角溢出部分。(文章后面会提供关于webkit-mask-image的相关介绍)
-webkit-mask-image 可以使用图片、Gradient 渐变或者 SVG mask 作为元素的 mask 遮罩。在 WebKit 的兼容性还算可以。


`解决 border-radius 元素在应用了 transform 的子元素 时overflow:hidden 失效的问题 - Lying二小 - 博客园`
http://www.cnblogs.com/lyingSmall/p/5337374.html


`父元素设置了overflow:hidden和border-radius，子元素超出部分不隐藏？ - 前端开发 - 知乎`
http://www.zhihu.com/question/31347571


`mask-image - CSS | MDN`
https://developer.mozilla.org/en-US/docs/Web/CSS/mask-image
