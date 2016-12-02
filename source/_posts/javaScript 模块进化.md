title: javaScript 模块进化
date: 2016-09-16 00:00:00
tags: [模块化]
 
---
# 最初的美好
- 污染全局变量
```
function bar(){
    //do something
}
function log(){
    //do something
}
```


- 减少了全局变量 &  暴露所有的模块成员，内部状态可以被外部读写
```
var myModule = {
    bar: function(){
        //do something
    },
    log: function(){
        //do something
    }
}
 
//调用
myMoudle.bar();
```


- 匿名闭包的写法（Immediately-Invoked Function Expression，IIFE）
```
var myModule = (function(){
    var _log = "hello world";
    var log = function(){
        console.log(_log);
    };
 
    return {
        log: log
    }
})();
 
//引用
myModule.log();//hello world
myModule._log;//undefined
```


---
-  引入依赖关系
```
var myModule = (function($){
    var $log = $('.log');
    var log = function(){
        console.log($log);
    };
 
    return {
        log: log
    }
})(jQuery);
 
//引用
myModule.log();
```


- 依赖管理
- 没有引入一个依赖的 js 文件而导致后面 js 的功能失效. | 请求过多 ，不利于性能优化
```
<script src="jquery.js"><script>
<script src="dialog.js"><script>
<script src="tooltip.js"><script>
<script src="toast.js"><script>
<script src="handlebar.js"><script>
......
```
---
#  CommonJS规范、AMD规范、CMD规范


##  CommonJS(代表作: Browserify )
-  服务器端的模块化的规范，是 Nood.js 在实践中推出的，Nood.js 也是首先采用 js 模块化的；
- exports 定义模块
- require 加载模块
```
//math.js
exports.add = function(a,b){
    var c = a + b;
    return c;
}
 
//index.js
var add = require('math').add;//
console.log(add(1,1));//2
```


## AMD(代表作:RequireJS )
- Asynchronous Module Definition，即异步的模块定义，是浏览器端的模块化规范，是 RequireJS 在推广过程中对模块定义的规范化产出


- define 定义模块(同时定义依赖),模块名为文件名
    - `JavaScript模块化－require.js，r.js和打包发布 - 简书` http://www.jianshu.com/p/7186e5f2f341
```
define(id?, dependencies?, factory);

```
```
define('myModule',['jQuery','types/Employee'],function($,Employee){//定义模块myModule，引入依赖jQuery，types/Employee
   function Programmer(){
            //do something
        };
        Programmer.prototype = new Employee();
        return Programmer;  //return Constructor
})
```


## CMD(代表作:SeaJS )
- Common Module Definition，即通用模块定义，是浏览器端的模块化规范，是 SeaJS 在推广过程中对模块定义的规范化产出


- seajs.use模块载入
    -  seajs.use('./a'),require('a'), require.asyc('./a.js')
    - `Seajs模块载入方法 - 简书` http://www.jianshu.com/p/2de0d5b24c8c
- define 定义模块(require提取依赖模块),模块名为文件名
    - `seajs模块加载机制 - 简书` http://www.jianshu.com/p/1245af09383e
```
define(factory)
```


- AMD 是 依赖关系前置，提前执行 ；CMD 是类似于 CommonJS 那样 按需加载，延迟执行 ：
```
//CMD recommanded
define(function(require, exports, module){
  var a = require('a');
  a.doSomething();
  var b = require('b');
  b.doSomething();    // 依赖就近，延迟执行
});
 
//AMD recommanded
define(['a', 'b'], function(a, b){
    a.doSomething();    // 依赖前置，提前执行
    b.doSomething();
});
```


--- 


# RequireJS模块化及r.js指南
- 在 html 中指定了 RrequireJS 第一次需要加载的模块，也就是通过 data-main 所指定
```
<!-- index.html -->
<script data-main="src/js/app/index" src="src/js/lib/require.js"></script>
```
-  index.js
```
requirejs.config({
  baseUrl: 'src/js',
  paths: {
    jquery: 'lib/jquery',
    backTop: 'com/backTop',
    jumpTo: 'com/jumpTo',
    carousel: 'com/carousel',
    exposure: 'com/exposure',
    navfloor: 'com/navfloor',
    waterfall: 'com/waterfall'
  }
});
```
- 其中 baseUrl 是改变基准路径，所有的模块相对于 baseUrl 来加载； baseUrl 路径以 index.html 所在的目录为基准；
- 其中 requirejs.config() 函数的 paths 属性指定各个模块的加载路径(相对于 baseUrl )，如果没有指定 baseUrl ，路径默认和主模块在同一目录;


采用了 RequireJS 模块化写法解决了命名冲突和依赖管理的问题，同时也增加了文件数量，这也意味着请求的增多，无疑会带来性能问题；



- 打包（优化）工具 r.js ,它可以实现前端文件的压缩与合并,减少服务器请求
```
({
    baseUrl: "./src/js",
    paths: {
        'jquery': 'lib/bower_components/jquery/dist/jquery.min'
    },
    name: "main",
    out: "dist/js/merge.js"
})
```
- baseUrl 路径设置应与主模块中 require.config() 方法里的 baseUrl 实际路径一致
- nam 指定的是解析入口，这里写了主模块，路径相对于前面指定的 baseUrl 


---
`前端模块化之旅（一）：因何生它 - 推酷`

http://www.tuicool.com/articles/z2AFRfF


`前端模块化之旅（二）：CommonJS、AMD和CMD - 推酷`
www.tuicool.com/articles/ZFvIfmz
 
`前端模块化之旅（三）：RequireJS模块化及r.js指南 - 推酷`
http://www.tuicool.com/articles/qyInuum


`Javascript模块化编程（一）：模块的写法 - 阮一峰的网络日志`
http://www.ruanyifeng.com/blog/2012/10/javascript_module.html



