title: javaScript 的值传递&闭包应用
date: 2016-10-10 00:00:00
tags: [ 闭包 ]


---
- 值传递
```
  var value = 1;
  var countValue;
 
  function count(value) {
    value = value + 1;// 值传递，引用断离
    countValue = value;
    console.log('count', value); // 2
  }
 
  console.log('value', value);// 1
  count(value);
  console.log('value', value);// 1
  console.log('value', countValue); // 2
```


- 闭包应用
```
  // 循环&异步&函数-问题
  for (var i = 0; i < 2; i++) {
    setTimeout(
      function () {
        console.log('>', i)
      }
    )
  }
 
  // 循环&异步的问题
  for (var i = 0; i < 2; i++) {
    (function (i) {
      // 函数接收的参数值属于值传递,引用断离
      setTimeout(
        function () {
          console.log('>>', i)
        }
      )
    })(i);
  }
```