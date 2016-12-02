title: ES6 模块化
date: 2016-09-15 00:00:00
tags: [ES6]
 
---
示例:  https://github.com/tildeio/rsvp.js  #downloads
- ES6编写
- 支持 CommonJS, AMD模块化规范
```
(function (global, factory) {
  typeof exports === 'object' && typeof module !== 'undefined' ? factory(exports) :// CommonJS
    typeof define === 'function' && define.amd ? define(['exports'], factory) : // AMD
      (factory((global.RSVP = global.RSVP || {}))); // window
}(this, (function (exports) {
  'use strict';
})));
```
---


现代浏览器对模块(module)支持程度不同， 目前都是使用babelJS， 或者Traceur把ES6代码转化为兼容ES5版本的js代码;



# ES6的模块化的基本规则或特点

```
//lib.js
//导出常量
export const sqrt = Math.sqrt;
//导出函数
export function square(x) {
    return x * x;
}
//导出函数
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}
 
//main.js
import { square, diag } from './lib';
console.log(square(11)); // 121
console.log(diag(4, 3)); // 5
```


# import和export的基本语法
- export{} 导出方式
```
//lib.js 文件
let bar = "stringBar";
let foo = "stringFoo";
let fn0 = function() {
    console.log("fn0");
};
let fn1 = function() {
    console.log("fn1");
};
export{ bar , foo, fn0, fn1}
 
//main.js文件
import {bar,foo, fn0, fn1} from "./lib";
console.log(bar+"_"+foo);
fn0();
fn1();
```
-  export{} 导出方式 + 别名
```
//lib.js文件
let fn0 = function() {
    console.log("fn0");
};
let obj0 = {}
export { fn0 as foo, obj0 as bar};
 
//main.js文件
import {foo, bar} from "./lib";
foo();
console.log(bar);
```
- 单行导出
```
//lib.js文件
export let foo = ()=> {console.log("fnFoo") ;return "foo"},bar = "stringBar";
 
//main.js文件
import {foo, bar} from "./lib";
console.log(foo());
console.log(bar);
```
- default匿名导出
```
//lib.js
export default "string";
 
//main.js
import defaultString from "./lib";
console.log(defaultString);
```
```
//lib.js
let fn = () => "string";
export {fn as default};
 
//main.js
import defaultFn from "./lib";
console.log(defaultFn());
```
- 通配符导出所有
```
//lib.js
export * from "./other";
//如果只想导出部分接口， 只要把接口名字列出来
//export {foo,fnFoo} from "./other";
 
//other.js
export let foo = "stringFoo", fnFoo = function() {console.log("fnFoo")};
 
//main.js
import {foo, fnFoo} from "./lib";
console.log(foo);
console.log(fnFoo());
```
```
import * as obj from "./lib";
console.log(obj);
```
# ES6导入的模块都是属于引用
```
//lib.js
export let counter = 3;
export function incCounter() {
    counter++;
}
export function setCounter(value) {
    counter = value;
}
 
 
//main.js
import { counter, incCounter ,setCounter} from './lib';
 
// The imported value `counter` is live
console.log(counter); // 3
incCounter();
console.log(counter); // 4
setCounter(0);
console.log(counter); // 0
```


# 循环依赖的问题
```
//index.html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8"/>
</head>
<body>
 
<script data-main="cyclic" src="//cdn.bootcss.com/require.js/2.2.0/require.min.js"></script>
<script>
//cyclic.js
require(["file0"], function(file0) {
    console.log(file0)
})
 
//file0.js
define(["file1"], function(file1) {
    console.log(file1)
    return {
        file0 : "file0"
    }
})
 
//file1.js
define(["file0"], function(file0) {
    console.log(file0);
    return {
        file1 : "file1"
    }
})
</script>
</body>
</html>
```
```
undefined
Object { file1: "file1" }
Object { file0: "file0" }
```
在执行file1.js的时候file0.js还没执行完， 所以输出了undefined， 这种输出结果和NodeJS输出的情况是一样的；





---
`ES6新特性：使用export和import实现模块化 - 方方和圆圆 - 博客园`
http://www.cnblogs.com/diligenceday/p/5503777.htm l
http://www.tuicool.com/articles/EryUniv


- ES6的模块化的基本规则或特点：
- 下面列出几种import和export的基本语法：
- ES6导入的模块都是属于引用：
- 循环依赖的问题：
- 浏览器兼容：
- 参考：



