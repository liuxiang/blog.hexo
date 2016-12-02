title: Safari 中使用sort差异
date: 2016-11-17 00:00:00
tags: [ Safari ]



---
# 排序场景
```
var myArray = [{date:"2013.03.01"},{date:"2013.03.08"},{date:"2013.03.19"}];


myArray.sort(function(a,b){
  return b.date > a.date;
})
```
- chrome
```
"2013.03.19", "2013.03.08", "2013.03.01"

```
- Safari 
```
"2013.03.01", "2013.03.08", "2013.03.19"

```


# 方法一 (兼容chrome,safari)
```
function(a,b){
  return b.date - a.date;
}
```


# 方法二(兼容chrome,safari.适合非数字类型的控制)
```
myArray.sort(function(a,b){
  return (b.date > a.date) ? 1 : (b.date < a.date) ? -1 : 0;
});
```
`javascript - Safari doesn't sort array of objects like others browsers - Stack Overflow`
https://stackoverflow.com/questions/15507729/safari-doesnt-sort-array-of-objects-like-others-browsers


---
# 解决过问题
```
$scope.openSection.knowPoints.sort(function (item,item2) {
  return (item.name == '本节总结') ? 1 : (item2.name == '本节总结')? -1 : 0; // 本章总结最后
});
```