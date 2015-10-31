title: angular——向目标controller发送对象(json 2 Str)数据
date: 2015-10-31 00:00:00 #发表日期，一般不改动
categories: 前端#文章文类
tags: [前端,angular,json] #文章标签，多于一项时用这种格式
photos:

---
# 利用JSON 2 String的方式,传递对象参数

>优点: 
可达到两个controller间的对象数据传递,且保持最短的生命周期. 
相比 local,service,数据能及时销毁,避免内存膨胀.

>缺点:
会将String的json对象显示到了地址栏,破坏了地址栏内容.
数据量太大会受到地址栏字符串长度限制.

控制器 A 
```
.controller('A-Ctrl',function ($scope, $stateParams) {
 $scope. request = [{id=1,name='xxx'}]; // 初始化待传递的数据对象 request
}

A.html
ui-sref="B-state({request-p:'{{request}}'}) <!-- 将json对象转换为String传递 -->
```
B  路由
```
  .state('B-state', {
    url: '/state/:request-p',             // 定义参数:request
    templateUrl: '**/B.html',
    controller: 'B-Ctrl'
  })
```
控制器 B
```
.controller('B-Ctrl',
['$scope', '$stateParams', function ($scope, $stateParams) {
 
  var request = JSON.parse($stateParams.request-p); // 将String格式化还原为JSON对象
 
  alert(JSON.stringify(request)); // String化预览
  alert(request.id);
}
```

# 附:AngularJS控制器controller如何通信？
## angular控制器通信的方式有三种：
1，利用作用域继承的方式。即子控制器继承父控制器中的内容
 
2，基于事件的方式。即$on,$emit,$boardcast这三种方式
 
3，服务方式。写一个服务的单例然后通过注入来使用

`AngularJS控制器controller如何通信？ - SegmentFault` 
http://segmentfault.com/a/1190000000639592


<!-- more -->