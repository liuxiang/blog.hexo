title: javaScript——json处理
date: 2015-10-29 00:00:00 #发表日期，一般不改动
categories:  前端 #文章文类
tags: [前端,javaScript,json] #文章标签，多于一项时用这种格式
photos:


---

# String  2  json
### 方法一 `eval()`
```
data = eval('('+ jsonString+')');
```
>eval 函数非常快，但是它可以编译任何 javascirpt 代码，这样的话就可能产生安全的问题。eval 的使用是基于传入的代码参数是可靠的假设的，有一些情况下，可能客户端是不可信任的。
 
如果基于安全的考虑的话，最好是使用一个 JSON 解析器。 一个 JSON 解析器将只接受 JSON 文本。所以是更安全的。


### 方法二 ` JSON.parse(jsonStr) `
```
data = JSON.parse(jsonStr);
```
>可选的 filter 参数将遍历每一个value key 值对， 并进行相关的处理。如：
```
myData = JSON.parse(text, function (key, value) {
     return key.indexOf('date') >= 0 ? new Date(value) : value;   
});
```


### 动态判断数据类型 转换为json
```
if (typeof result == 'object') {
     data = result;
} else {
     data = JSON.parse(result);
}
```


# json 2 String


```
var jsonStr = JSON.stringify(jsonObj);

```


---
# json 对象依据条件分组成两个对象 & json动态叠加


```javaScript
var bags = [{id=1},{ id=2 },{ id=3 }];


var courseHaves;
var courseNots;
for (var i in bags) {
 
  var bags_s = JSON.stringify(bags[i]);
 
  if (bags[i].id == 1) {
    courseHaves = (courseHaves == null) ? bags_s : courseHaves.concat(","+bags_s);
  } else {
    courseNots = (courseNots == null) ? bags_s : courseNots.concat(","+bags_s);
  }
}
 
$scope.courseHaves = JSON.parse("["+courseHaves+"]");
$scope.courseNots = JSON.parse("["+courseNots+"]");


// alert(JSON.stringify($scope.courseHaves)+"|"+$scope.courseHaves.length);
// alert(JSON.stringify($scope.courseNots)+"|"+$scope.courseNots.length);
```


<!-- more -->
