title: js值传递,引用传递,引用断离
date: 2016-11-23 00:00:00
tags: [ js对象传递 ]



---
# 引用传递
```
var a={a:1};
var b=a;
b.b=1;
console.log(b);// Object {a: 1, b: 1}
console.log(a);// Object {a: 1, b: 1}
```


# 引用断离
```
var a={a:1};
var b=JSON.parse(JSON.stringify(a));
b.b=1;
console.log(b);// Object {a: 1, b: 1}
console.log(a);//  Object {a: 1}
```