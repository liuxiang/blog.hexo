
title: javaStript Tips
date: 2016-05-20 00:00:00
tags: [  javaStript,Tips ]


---


## 光标焦点总在最后
```
onclick="var $Val = $.trim($(this).val());$(this).val('').focus().val($Val)"
```
https://segmentfault.com/q/1010000003843756/a-1020000003847663
 
## 随机数取整
```
console.log(1.2,1.6);    //  1.2 1.6
console.log(0|1.2,0|1.6); //  1 1
console.log(~~1.2,~~1.6); //  1 1
```



# input输入替换
onkeyup="value=value.replace(/[^\\d]/g,\'\').substring(0,5)"
```
<input class="form-control input-sm-pages" onkeyup="value=value.replace(/[^\d]/g,'').substring(0,5)" type="text" size="2.6" value="1" title="输入跳转页码，回车确认">
```


#jQuery Lazy Load 图片延迟加载

```
<script src="jquery.js"></script>
<script src="jquery.lazyload.js"></script>


<!--
将真实图片地址写在 data-original 属性中，而 src 属性中的图片换成占位符的图片（例如 1x1 像素的灰色图片或者 loading 的 gif 图片）
添加 class="lazy" 用于区别哪些图片需要延时加载，当然你也可以换成别的关键词，修改的同时记得修改调用时的 jQuery 选择器
添加 width 和 height 属性有助于在图片未加载时占满所需要的空间
-->
<img class="lazy" src="grey.gif" data-original="example.jpg" width="640" heigh="480">


$('img.lazy').lazyload();

```
`jQuery Lazy Load 图片延迟加载 - 前端开发仓库`
http://code.ciaoca.com/jquery/lazyload/


# 箭头函数
```
var fn = (param) => {console.log('hello '+ param)};
fn('world');
```


# 尾递归
- 尾调用是指某个函数的最后一步是调用另一个函数
- 函数调用自身，称为递归
- 如果尾调用自身，就称为尾递归


递归很容易发生"栈溢出"错误（stack overflow）
```
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}


factorial(5) // 120
```

`5*4*3*2*1`



但对于尾递归来说，由于只存在一个调用记录，所以永远不会发生"栈溢出"错误

```
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}
 
factorial(5, 1) // 120
```
`(5,1)<=(4,5)<=(3,4*5)<=(2,3*4*5)<=(1,2*3*4*5)==1*2*3*4*5`



柯里化减少参数
```
function currying(fn, n) {
  return function (m) {
    return fn.call(this, m, n);
  };
}
 
function tailFactorial(n, total) {
  if (n === 1) return total;
  return tailFactorial(n - 1, n * total);
}
 
const factorial = currying(tailFactorial, 1);
 
factorial(5) // 120
```


反柯里化
```
Function.prototype.uncurry = function () {
  return this.call.bind(this);
};
```


# JS 写函数类时加NEW与不加NEW的区别
定义一个类，如下，然后NEW一个，调用他里面的方法与属性，
```
function showa(s)
{
        this.color="red";//公有变量
        this.size="9";
        this.txt=s;
        this.show=function() //类的方法
        {
               alert(this.color);
        }
}
```
然后NEW一个调用，
```
var sa=new showa("abc");
alert(sa.size);
sa.show();
```
弹出“9”，　和“red”,
改成以下这样的没有NEW的
```
var sa=showa("abc");
alert(sa.size);
sa.show();
```
提示 sa  没的定义 
JS有些函数不加NEW和加NEW，执行都是一样的，在网上查下看了些概念太理解，可以通俗地说下吗？


```
new声明的是一个对象，而不是函数
而直接写函数，那就不是对象，是无法调用对象的属性的！
```


# 隐性参数列表 arguments
```
function fn(){
    console.log("arguments",arguments);// arguments ["hello", "world"]
}
 
fn("hello","world");
```


# call & apply
call，apply都是改变函数执行时的上下文，即this的环境。常结合`call & inherits`实现对象继承
```
function inherits(child, parent) {
  var _proptotype = Object.create(parent.prototype);
  _proptotype.constructor = child.prototype.constructor;
  child.prototype = _proptotype;
}
```
`区别` 参数使用方式不同

```
fn.call(context, arg1, arg2, …, argn)
fn.apply(context, [arg1, arg2, …, argn])
```
独立的`call`会使上下文覆盖。如需继承需结合`inherits(child, parent)`拷贝属性
```
function Animal(){   
    this.name = "Animal";   
this.name2 = "Animal2";   
    this.showName = function(){   
        console.log(this.name,this.name2);   
    }   
}   
 
function Cat(){   
    this.name = "Cat";   
}   
 
var animal = new Animal();   
var cat = new Cat();   
 
//通过call或apply方法，将原本属于Animal对象的showName()方法交给对象cat来使用了。   
//输入结果为"Cat"   
animal.showName();// Animal Animal2
animal.showName.call(cat,",");// Cat undefined
```
`JS中的call()和apply()方法 - 每天进步一点点！`
http://uule.iteye.com/blog/1158829



# bind
fn2对象载入fn对象（this）
```
fn.bind(fn2);
```
```
altwrite.bind(document)("hello"); 



```
```
var altwrite = function(){this.name='aaa'}
altwrite.bind(document).name;// aaa
altwrite.bind(document).write('hello')// is not a function
---
var altwrite = document.write;
altwrite('hello');// Uncaught TypeError: Illegal invocation （this指向被改变为了window）
altwrite.bind(document)('hello'); // altwrite改变this的指向为document(即:原this内容无法访问)
altwrite.call(document, "hello");  // altwrite对象上下文由document对象上下文取代(即:原this内容无法访问)

```
```
this.num = 9;
var mymodule = {
  num: 81,
  getNum: function() { return this.num; }
};
 
module.getNum(); // 81
 
var getNum = module.getNum;
getNum(); // 9, 因为在这个例子中，"this"指向全局对象
 
// 创建一个'this'绑定到module的函数
var boundGetNum = getNum.bind(module);
boundGetNum(); // 81
```
`Javascript中bind()方法的使用与实现 - Superlin's Blog `
https://segmentfault.com/a/1190000002662251



```

var obj = {
    x: 81,
};
 
 
// var x='88',xx='99';
var foo = {
    x :'88',
    xx : '99',
    getX : function() {
        console.log(this.x,this.xx);
    }
}
 
console.log(foo.getX());            // 88,99
console.log(foo.getX.bind(obj)());  // 81,undefined
console.log(foo.getX.call(obj));    // 81,undefined
console.log(foo.getX.apply(obj));   // 81,undefined
```
`chokcoco/apply-call-bind: 深入浅出妙用Javascript中apply、call、bind`
https://github.com/chokcoco/apply-call-bind/tree/master



# bind()捷径
bind()也可以为需要特定this值的函数创造捷径。
 
例如要将一个类数组对象转换为真正的数组，可能的例子如下：
```
var slice = Array.prototype.slice;
 
// ...
 
slice.call(arguments);
```
如果使用bind()的话，情况变得更简单：
```
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.call.bind(unboundSlice);
 
// ...
 
slice(arguments);
```


# 在javascript中有三种声明变量的方式：var、let、const
```
var：声明全局变量

let：声明块级变量，即局部变量.
   - (注意：必须声明'use strict';后才能使用let声明变量否则浏览并不能显示结果)
const：用于声明常量，也具有块级作用域 
```
http://blog.csdn.net/u012786716/article/details/50740710






---


`JavaScript 精粹 - 推酷`
http://www.tuicool.com/articles/Mv6zUr
 
`50 tips of JavaScript，这些坑你都知道吗？`
http://www.tuicool.com/articles/MZfuA3r


`12个JavaScript技巧 - 推酷`
http://www.tuicool.com/articles/uyMNraF



<!-- more -->

