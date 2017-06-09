title: javaScript 循环遍历
date: 2016-01-16 00:00:00
categories: javaScript 循环遍历
tags: [javaScript 循环遍历]
 
---
 
# `集合/数组` 遍历
```
var numbers = [1, 2, 3];
 
for(var i=0;i<numbers.length;i++){
  console.log('Index ' + i + ' with number ' + numbers[i]);
}
 
for(var i in numbers){
  console.log('Index ' + i + ' with number ' + numbers[i]);
}
 
numbers.forEach(function(number, index) {
  console.log('Index ' + index + ' with number ' + number);
});
```
```
var arr = ['a', 'b', 'c'];
for(data in arr) {
    console.log(data);// 0 1 2
}


var arr = ['a', 'b', 'c'];
for(data of arr) {
    console.log(data);// a b c
}
```
 
#` 对象/json` 遍历
```
var xx = {a:'a+', b: 'b+'}
for (var key in xx) {
  console.log(key+" "+xx[key]);
}
```
注意: json无`length`属性
```
var anObject = {one:1,two:2,three:3};//对json遍历each 
$.each(anObject,function(name,value) { 
console.log(name, value ); 
});

```


# `集合/数组`  转 `对象/json` 转 `集合/数组`
```
var array = [{id:'id-1',a:'a+'}, {id:'id-2',b:'b+'}, {id:'id-3',c:'c+'}];
var json = {};
array.forEach(function(value) {
  console.log(value);
  json[value.id] = value;
});
 
for(var key in json){
  console.log(key,json[key]);
}
 
/***********************************************/
var array_new=[];
for(var key in json){
  array_new.push(json[key]);
}
 
array.forEach(function(value) {
  console.log(value);
});
```
 
# 删除`集合/数组`中指定元素
```
var xx = ['a','b','c'];
xx.splice(xx.indexOf('b'),1);
console.log(xx);
```
 
# 删除`对象/json`中指定元素
```
var json = {a:'a+'};
console.log(json);
delete json['a'];
console.log(json);
```
---
# angular遍历
- array数组
```
var objs=[{a:1},{a:2}];
angular.forEach(objs, function(data,index,array){
console.log(data.a+'='+array[index].a);
});
VM584:3 1=1
VM584:3 2=2
```
- json对象
```
var objs={a:1,b:2};
angular.forEach(objs, function (value, key) {
    console.log(key,value);
});
VM596:3 a 1
VM596:3 b 2
```
```
var objs={a:1,b:2};
angular.forEach(objs, function (value, key,data) {
    console.log(data, key,value);
});
VM595:3 Object {a: 1, b: 2} "a" 1
VM595:3 Object {a: 1, b: 2} "b" 2
```


- 删除空json
```
var objs={'a':null,'b':'bb'};
console.log('show',objs);
angular.forEach(objs, function (value, key,data) {
       !value && delete objs[key];
});
console.log('result',objs);
```


---


**参考**
`JavaScript对Json的增删改属性 - xerxes的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/shangliuyan/article/details/8072257
 
---
```
var lis = document.querySelectorAll('li');
for(var i=0;i<lis.length;i++){
    if(lis[i].style.cssFloat=="left"){
       lis[i].style.cssFloat="right";
    }else if(lis[i].style.cssFloat=="right"){
        lis[i].style.cssFloat="left";
    }
}
 
同时说明一下，对于上面的lis不能够使用foreach()，因为lis是NodeList,不是List，也不是数组，不能使用for-each循环。
```
`如何循环遍历document.querySelectorAll方法返回的结果`
https://zhidao.baidu.com/question/1756737727649600028.html


`深入了解JavaScript中的for循环 - 推酷`
http://www.tuicool.com/articles/BVzEFbM


<!-- more -->
