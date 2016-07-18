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
 
#` 对象/json` 遍历
```
var xx = {a:'a+', b: 'b+'}
for (var key in xx) {
  console.log(key+" "+xx[key]);
}
```
注意: json无`length`属性
 
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
**参考**
`JavaScript对Json的增删改属性 - xerxes的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/shangliuyan/article/details/8072257
 
<!-- more -->
