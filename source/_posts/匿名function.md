title:  匿名function
date: 2016-03-26 00:00:00
categories:
tags: [匿名 function ]


---
#  var functionName = function() {} vs function functionName() {}
```
<script>
  // Error
  functionOne();
 
  var functionOne = function() {
  };
</script>
```
```
<script>
  // No error
  functionTwo();
 
  function functionTwo() {
  }
</script>
```


**参考**
`javascript - var functionName = function() {} vs function functionName() {} - Stack Overflow`
http://stackoverflow.com/questions/336859/var-functionname-function-vs-function-functionname


`[JavaScript] var x = (function(){...})(); 和 var x = (function(){...}()); 两种写法一样的吗？有什么区别？ - JavaScript - 知乎`
http://www.zhihu.com/question/19692558


`JavaScript 中，定义函数时用 var foo = function () {} 和 function foo() 有什么区别？ - 前端开发 - 知乎`
http://www.zhihu.com/question/19878052


`JavaScript 中对变量和函数声明的“提前（hoist）`
http://www.bootcss.com/article/variable-and-function-hoisting-in-javascript/


<!-- more -->
