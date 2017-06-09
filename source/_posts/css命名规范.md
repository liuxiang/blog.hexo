title: css命名规范
date: 2017-2-13 00:00:02
tags: [ 前端 规范 ]


---
# CSS 设计模式
- OOCSS
减少对 HTML 结构的依赖
增加 CSS class 重复性的使用
- SMACSS
前对 SMACSS 的概念仅限于它对 CSS 不同的业务逻辑所做的划分方式



---
# CSS 规范
- BEM
- Aomic


---
# BEM命名规范

BEM的意思就是块（block）、元素（element）、修饰符（modifier）。


- block: 
可以理解为一个区域、一个组件或者一个块级元素，具体如何区分需要根据实际情况具体分析；
- block__element： 
就是一个上面的block里面的元素，比如说导航（nav：block）里面有a标签（a: element）就是一个元素， block与element使用两个下划线链接；
- block__element–modifier： 
我的理解是状态或属性。比如element里面的a标签，它有active、hover、normal三种状态，这三种状态就是modifier。midifier是使用两个“–”中横线连接


```
.block 代表某个基本的抽象元素；
.block__element 代表 .block 这一整体的一个子元素；
.block--modifier 代表 .block 的某个不同状态。
```


## 示例
```
<!-- HTML结构 -->
<nav class="nav">
  <a href="#" class="nav__item nav__item--active">当前页：active</a>
  <a href="#" class="nav__item nav__item--hover">假设鼠标在这里要加个hover的class</a>
  <a href="#" class="nav__item nav__item--normal">假设需要加个normar的状态</a>
</nav>
```
```
// SASS写法
.nav{
  &__item{
    &--active{
      <!-- active样式 -->
    }
    &--hover{
      <!-- hover样式 -->
    }
    &--normal{
      <!-- normal样式 -->
    }
  }
}
```
```
/* 编译后的css */
.nav{ }
.nav__item{ }
.nav__item--active{ }
.nav__item--hover{ }
.nav__item--normal{ }
```


`一些编写css的建议 - 推酷`
http://www.tuicool.com/articles/JFRVraa


---
# 调查报告
```

Q8-CSS 方法论与命名空间使用体验
下一个问题是关注开发者对于下列所有的CSS常见方法论与命名空间的使用体验:
SMACSS

Object 

Atomic Design

ITCSS

CSS Modules

BEM

SUIT CSS



在上表可以看出，BEM算是目前最受欢迎，也是使用体验最好的CSS命名方案之一，基本上在真实的使用者中超过了50%的人表示很满意。不过令我震惊的是，对于OOCSS等常见的CSS方法论有所了解的开发者数量占比仅有不到30%，即使在那些Advanced或者Expert级别的开发者中，没有任何一项方法论的使用度超过了20%。
```

`2016 年前端开发者深度调研，看看别人使用什么技术体系 - 推酷`
http://www.tuicool.com/articles/mUveAbe


---


# 模块化
- css模块文件：`module/aboutUs.scss`

```
@charset "utf-8";


//关于我们
.page-aboutUs{
  .about-us {
    text-align: center;
    padding: 10%;
    .wosai-logo img {
      width: 80px;
      height: 80px;
      @include border-radius(5px);
    }
    .app-version{
      font-size: 16px;
      color: #b0b2b7;
    }
    

    ...



}
```
-  css应用文件：` style.main.scss`
```
@charset "utf-8";

@import "module/aboutUs";//-----------------------关于页面

```
- 配合IDE或gulp的`css watchers`功能，输出`style.main.css`文件


---
**参考**
`CSS 命名规范总结` 
http://www.tuicool.com/articles/QN7rAbN



`Css设计模式-理论篇之OOCSS、SMACSS与BEM - 推酷`
http://www.tuicool.com/articles/QNBFniY


`CSS设计模式：OOCSS 和 SMACSS - 新闻 - SegmentFault`
https://segmentfault.com/a/1190000000389838


`编写可维护的CSS - 新闻 - SegmentFault`
https://segmentfault.com/a/1190000000388784


`提升你的 CSS 姿势 - 推酷`

http://www.tuicool.com/articles/7fmY32j





