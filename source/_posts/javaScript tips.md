
title:  javaScript t ips
date: 2016-05-20 00:00:00
tags: [  javaScript , t ips ]


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






# 箭头函数
```
var fn = (param) => {console.log('hello '+ param)};
fn('world');
```


---
#  柯里化 ( Currying )
- 柯里化有3个常见作用：1. 参数复用；2. 提前返回；3. 延迟计算/运行

```
function add(a, b) {
    return a + b;
}


function curryingAdd(a) {
    return function(b) {
        return a + b;
    }
}


add(1, 2); // 3
curryingAdd(1)(2); // 3
```


## `IIFE`的`Currying`实现
`IIFE ` javascript立即执行函数表达式 
语法：` (function(){ /* code */ })(); `
```
(function(param){
  console.log(param);
})('hello')
```
`Currying`实现
```
function currying(fn){
  return function(param){
    fn(param);
  }
}


currying(function(param){
  console.log(param);
})('hello');
```


## 柯里化(Currying)减少参数
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


## 柯里化还能循环中闭包使用timeout
```
  /**
   * 异步执行函数,目的:页面同步刷新(柯里化)
   * @param func
   * @param time
   * @returns {Function}
   */
  function asyn(func, time = 1000) {
    return function (prom) {
      setTimeout(function () {
        return func.call(this, prom);
      }, time)
    }
  }


function _forEach(start, end) {
  asyn(function (prom) {
    // console.log('dd', prom);
    el('.nav-bar-title.ng-binding').triggerHandler('click');
    asyn(function (prom) {
      // console.log('dd2', prom);
      el('.bookname_content>.bookname', prom).triggerHandler('click');
      ++start <= end && dd(start, end);// 递归调用自己,相比for没有了时间计算
    })(prom);
  })(start);
}


_forEach(0, 2);// 启动
```


## 反柯里化
```
Function.prototype.uncurry = function () {
  return this.call.bind(this);
};
```


`掌握JavaScript函数的柯里化 - angular - SegmentFault`
https://segmentfault.com/a/1190000006096034


`浅析 JavaScript 中的 函数 currying 柯里化 - Tong Zeng - 博客园`
http://www.cnblogs.com/zztt/p/4142891.html


`JS中的柯里化(currying) « 张鑫旭-鑫空间-鑫生活`
http://www.zhangxinxu.com/wordpress/2013/02/js-currying/  


`由浅入深，深入详解 JavaScript 柯里化 - 推酷 ` -  柯里化与bind
http://www.tuicool.com/articles/y6BVvqq


---
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



#  JavaScript中匿名函数的递归调用
callee 是 arguments 对象的一个属性，其值是当前正在执行的 function 对象。 它的作用是使得匿名 function 可以被递归调用。 下面以一段计算斐波那契序列（Fibonacci sequence）中第 N 个数的值的代码来演示 arguments.callee 的使用
```
function fibonacci(num) {
    return (function(num) {
        if (typeof num !== "number") return -1;
        num = parseInt(num);
        if (num < 1) return -1;
        if (num == 1 || num == 2) return 1;
        return arguments.callee(num - 1) + arguments.callee(num - 2);
    })(num);
}
fibonacci(100);
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
function fn(a,b,c){
    console.log("arguments",arguments);// arguments ["hello", "world"]
}
 
fn("hello","world");
```
```
  function fn(func, b, c) {
    var _arguments = arguments;
    return function (src) {
      func.apply(null, _arguments);// 传递参数
    }
  }


var func =  fn(alert, 2,3);
func ("world");
```




# call & apply
call，apply都是改变函数执行时的上下文，即this的环境。常结合`call & inherits`实现对象继承
```
function inherits(child, parent) {
  var _proptotype = Object.create(parent.prototype); // 父级原型
  _proptotype.constructor = child.prototype.constructor;// 子集构造,覆盖父级构造
  child.prototype = _proptotype; // 父级原型覆盖子集原型
}
```
`JavaScript 精粹 - 推酷`
http://www.tuicool.com/articles/Mv6zUr


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


# 快捷逻辑
```
true  && console.log('1');// 1
false && console.log('2'); 
 
true  || console.log('3');
false || console.log('4');// 4
```


#  延展操作符(Spread operator) 
> （类似知识点： 隐性参数列表 arguments）


这个 … 操作符（也被叫做延展操作符 － spread operator）已经被 ES6 数组 支持。它允许传递数组或者类数组直接做为函数的参数而不用通过apply。


```
function a(...args){
    console.log('a',args); // a [1, 2, 3]
    b(args); // b [1, 2, 3] undefined undefined
    b(...args); // b 1 2 3
}
 
function b(a,b,c){
    console.log("b",a,b,c);
}
 
a(1,2,3);
```


# ES6字符串模板 Template Literals（模板对象） in ES6
```
function ss() {
  var service = {
    _kingRank: `/acticity/question/kingRank/${userId}/?pageSize=:pageSize&currentPage=:currentPage`,
    _friendsHelp: '/acticity/question/friendsHelp/:userId/?pageSize=:pageSize&currentPage=:currentPage'
  }
  return service;
}
var userId = 2; // 变量提升了
console.log(ss()._kingRank);// /acticity/question/kingRank/2/?pageSize=:pageSize&currentPage=:currentPage
```
```
function ss() {
  var service = {
    _kingRank: `/acticity/question/kingRank/${userId}/?pageSize=:pageSize&currentPage=:currentPage`,
    _friendsHelp: '/acticity/question/friendsHelp/:userId/?pageSize=:pageSize&currentPage=:currentPage'
  }
  return service;
}
function ff(){
  var userId = 2;
  console.log(ss()._kingRank);// error 找不到 userId
}
ff();
```


# 程序断点
Chrome会自动在插入它的地方停止

```
if (thisThing) {
    debugger;
}
```


# 快速定位调试函数 `debug(funcName)`
当我们想在函数里加个断点的时候，一般会选择这么做：
  1.在Inspector中找到指定行，然后添加一个断点
  2.在脚本中添加一个debugger调用
 
不过这两种方法都存在一个小问题就是都要到对应的脚本文件中然后再找到对应的行，这样会比较麻烦。这边介绍一个相对快捷点的方法，就是在console中使用debug(funcName)然后脚本会在指定到对应函数的地方自动停止。这种方法有个缺陷就是无法在私有函数或者匿名函数处停止，所以还是要因时而异：
```
var func1 = function() {
func2();
};
 
var Car = function() {
this.funcX = function() {
this.funcY();
}
 
this.funcY = function() {
this.funcZ();
}
}
 
var car = new Car();
```

 
# JavaScript截取字符串的Slice、Substring、Substr函数详解和比较
```
'第七章力'.slice(0,-1);// 第七章
'第七章力'.substr(0,-1);// ""
'第七章力'.substring(0,-1); // ""
 
'第七章力'.slice(0,'第七章力'.indexOf('章')+1);//  第七章
'第七章力'.substr(0,'第七章力'.indexOf('章')+1);//  第七章
```
`JavaScript截取字符串的Slice、Substring、Substr函数详解和比较_javascript技巧_脚本之家`
http://www.jb51.net/article/48257.htm


`JavaScript 数组：对比 slice 与 splice - 推酷`
http://www.tuicool.com/articles/MZ77Nvz



# 除自己,选择姐妹标签(siblings)
```

$('.navbar_rank').on('click', '.switch', function () {
     $(this).addClass('activity').siblings('.activity').removeClass('activity');
});
```
- weiui css
```

$('.shrink-items').on('click', '.shrink-item', function () {
  if ($(this).hasClass('active')) {
    $(this).removeClass('active');// 已展开,收起来
  } else {
    $(this).addClass('active').siblings('.active').removeClass('active');// 切换展开
  }
});
```

等效
```

$('.shrink-item').on('click', function () {
  if ($(this).hasClass('active')) {
    $(this).removeClass('active');// 已展开,收起来
  } else {
    $(this).addClass('active').siblings('.active').removeClass('active');// 切换展开
  }
});
```


#  prototype原型扩展,与方法内预定义
```

function People(name, age) {
  this.u_name = name;
  this.u_age = age;
}
 
People.prototype.getName = function () {
  return this.u_name;
}
 
dir(People);
dir(new People(2));
console.log(new People(3).getName());// 3
```

```
function People(name, age) {
    this.u_name = name;
    this.u_age = age;
 
    function getName() {
        return this.u_name;
    }
}
 
dir(People);
dir(new People(2));
console.log(new People(3).getName());// Uncaught TypeError: (intermediate value).getName is not a function


```

```

function People(name, age) {
    this.u_name = name;
    this.u_age = age;
 
    this.getName = function() {
        return this.u_name;
    }
}
 
var p = new People(4)
console.log(p.getName());// 4
```

或
```

function People(name, age) {
 
    function getName() {
        return this.u_name;
    }
 
    return {
        u_name: name,
        u_age: age,
        getName: getName
    };
 
}
 
var p = new People(4)
console.log(p.getName()); // 4
```


# 强制重排,重绘

```
// 对面板强行重绘（解决文字重叠问题）
$("ion-content").css('display', 'none');
document.body.offsetWidth;// 在样式变化间加入计算,可强制提交之前的样式变化。否则浏览器会进行事务优化，事务前后无变化的不再重绘
$("ion-content").css('display', '');
```


# 删除数组
- 使用`delete` 

【非gangular(ng-repeat)环境,不能动态更新已展示元素的index下标】

```
var a=['a','b',' c '];
delete a[0];
a;// [undefined × 1, "b", "c"]
```

- 使用`splice` (推荐)
```

var a=['a','b','c'];
a.splice(0,1);// 参数一：位置，参数二：删除数量
a;//  ["b", "c"]
```
- 使用过滤器
```
var arr = [2, 3, 5, 7];
arr = arr.filter(item => item !== 5);
```


---
# 循环中删除数组
- 循环中删除
```
var a=[1,2,3,4,5,6];
a.forEach(function(item,index){
    // console.log(item,index);
    if(item == 2 || item == 3){
        console.log('remove',index);
        // a.splice(index,1);// 存在实时移位造成数据跳过的问题
        delete a[index];// 保留位置删除，避免实时移位问题
    }
})
console.log(a);// [1, undefined × 2, 4, 5, 6]
a = a.filter(item => item !== 'undefined');// 清楚null值
console.log(a);// [1, 4, 5, 6]
```
- 逆向遍历
```
var arr = [2, 3, 5, 7];
for (let i = arr.length - 1; i >= 0; i--) {
    if (arr[i] === 5) {
        arr.splice(i, 1);
    }
}
```


`js 如何一次性删除数组中的多个元素？`
https://segmentfault.com/q/1010000006956973?_ea=1190266



`JavaScript splice() 方法`
http://www.w3school.com.cn/jsref/jsref_splice.asp


---


`JavaScript 精粹 - 推酷`
http://www.tuicool.com/articles/Mv6zUr
 
`50 tips of JavaScript，这些坑你都知道吗？`
http://www.tuicool.com/articles/MZfuA3r


`12个JavaScript技巧 - 推酷`
http://www.tuicool.com/articles/uyMNraF



`JavaScript调试十个小Tips - 推酷`
http://www.tuicool.com/articles/RnmA3mR


`搜索 JavaScript Tips - 推酷`

http://www.tuicool.com/search?kw=JavaScript+Tips


`console 让 js 调试更简单 - 推酷`
http://www.tuicool.com/articles/ymm6nan


`Web程序员必须知道的 Console 对象里的九个方法 - 推酷`
http://www.tuicool.com/articles/bANryqF


<!-- more -->

