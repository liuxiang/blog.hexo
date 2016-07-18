title: javaScript IIFE & 避免冲突
date: 2016-3-3 00:00:03
categories:   javaScript  

tags: [ javaScript,  IIFE ]


---


# 关键字
`命名空间` ` 全局变量` ` 冲突` `污染`


# 全局命名空间污染与 IIFE
总是将代码包裹成一个 IIFE(Immediately-Invoked Function Expression)，用以创建独立隔绝的定义域。这一举措可防止全局命名空间被污染。
>IIFE 还可确保你的代码不会轻易被其它全局命名空间里的代码所修改（i.e. 第三方库，window 引用，被覆盖的未定义的关键字等等）。


# 作用
javascript中没用私有作用域的概念，如果在多人开发的项目上，你在全局或局部作用域中声明了一些变量，可能会被其他人不小心用同名的变量给覆盖掉， 那么这时候就可以使用IIFE创建一个私有作用域来保护变量，防止命名冲突，俗称“匿名包裹器”或“命名空间”。


---
**参考**
`javascript 这样写(function(){})() 有什么用途？`
https://segmentfault.com/q/1010000000718015


`立即执行函数: (function ( ){...})( ) 与 (function ( ){...}( )) 有什么区别?`
https://segmentfault.com/q/1010000000442042


`如何让js不产生冲突，避免全局变量的泛滥，合理运用命名空间`
http://www.cnblogs.com/fengyv/p/3789414.html


`在JavaScript中创建命名空间的几种写法`

http://www.admin10000.com/document/4513.html
http://ourjs.com/detail/538d8d024929582e6200000c


`dongshaohan/IIFE: IIFE立即执行函数详解`
https://github.com/dongshaohan/IIFE



`mariusschulz/gulp-iife: A Gulp plugin for wrapping JavaScript code in IIFEs.`
https://github.com/mariusschulz/gulp-iife


`[译] JavaScript：立即执行函数表达式（IIFE）`
https://segmentfault.com/a/1190000003985390



`这几种立即执行函数表达式（IIFE）有什么区别？`
https://segmentfault.com/q/1010000004313085


`Javascript知识点：IIFE - 立即调用函数`
https://segmentfault.com/a/1190000000523767



<!-- more -->


