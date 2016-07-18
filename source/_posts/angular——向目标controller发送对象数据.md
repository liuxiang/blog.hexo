title: angular——向目标controller发送对象数据
date: 2015-10-31 00:00:00 #发表日期，一般不改动
categories:  前端 #文章文类
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

.controller('A-Ctrl', function ($scope, $stateParams) {

  $scope.  request = [{id=1,name='xxx'}]; // 初始化待传递的数据对象 request
}


A.html
ui-sref=" B- state ({ request-p :'{{ request }}'}) <!-- 将json对象转换为String传递  -->
```
B  路由

```
  .state('B- state ', {
    url: '/ state/: request-p ',                 // 定义参数:request
    templateUrl: '**/B.html',
    controller: ' B-Ctrl '
  })
```
控制器 B
```
.controller('B-Ctrl',
['$scope', '$stateParams', function ($scope, $stateParams) {
 
  var request = JSON.parse($stateParams. request-p ); //  将String格式化还原为JSON对象
 
  alert(JSON.stringify(request)); // String化预览
  alert(request.id);
}
```
<!-- more -->

---


# UI-Router 如何传对象

## 解决器 Resolve
可以使用resolve为控制器提供可选的依赖注入项。
resolve配置项是一个由key/value构成的对象。
key – {string}：注入控制器的依赖项名称。
factory - {string|function}：
string：一个服务的别名
function：函数的返回值将作为依赖注入项，如果函数是一个耗时的操作，那么控制器必须等待该函数执行完成（be resolved）才会被实例化。
在controller实例化之前，resolve中的每一个对象都必须 be resolved，请注意每个 resolved object 是怎样作为参数注入到控制器中的。


## 示例
```
$stateProvider.state('myState', {
      resolve:{
 
         // Example using function with simple return value.
         // Since it's not a promise, it resolves immediately.
         simpleObj:  function(){
            return {value: 'simple!'};
         },
 
         // Example using function with returned promise.
         // 这是一种典型使用方式
         // 请给函数注入任何想要的服务依赖，例如 $http
         promiseObj:  function($http){
            // $http returns a promise for the url data
            return $http({method: 'GET', url: '/someUrl'});
         },
 
         // Another promise example.
         // 如果想对返回结果进行处理， 可以使用 .then 方法
         // 这是另一种典型使用方式
         promiseObj2:  function($http){
            return $http({method: 'GET', url: '/someUrl'})
               .then (function (data) {
                   return doSomeStuffFirst(data);
               });
         },       
 
         // 使用服务名的例子，这将在模块中查找名称为 'translations' 的服务，并返回该服务
         // Note: The service could return a promise and
         // it would work just like the example above
         translations: "translations",
 
         // 将服务模块作为解决函数的依赖项，通过参数传入
         // 提示：依赖项 $stateParams 代表 url 中的参数
         translations2: function(translations, $stateParams){
             // Assume that getLang is a service method
             // that uses $http to fetch some translations.
             // Also assume our url was "/:lang/home".
             translations.getLang($stateParams.lang);
         },
 
         // Example showing returning of custom made promise
         greeting: function($q, $timeout){
             var deferred = $q.defer();
             $timeout(function() {
                 deferred.resolve('Hello!');
             }, 1000);
             return deferred.promise;
         }
      },
 
      // 控制器将等待上面的解决项都被解决后才被实例化
      controller: function($scope, simpleObj, promiseObj, promiseObj2, translations, translations2, greeting){
 
          $scope.simple = simpleObj.value;
 
          // 这里可以放心使用 promiseObj 中的对象
          $scope.items = promiseObj.items;
          $scope.items = promiseObj2.items;
 
          $scope.title = translations.getLang("english").title;
          $scope.title = translations2.title;
 
          $scope.greeting = greeting;
      }
   })
```


示例2:
```
@scope.toOrder = function(){
    $state.go('choosePay',{plan:$scope.planSelected});
}


.state('choosePay',{
    url: '/choosePay',
    params: {plan:null},
    templateUrl : '***.html',
    controller: 'ChoosePayCtrl'
})


controller: function($scope,$stateParams){
    $scope.plan = $stateParams.plan;
})
```




# 附:AngularJS控制器controller如何通信？
## angular控制器通信的方式有三种：
1，利用作用域继承的方式。即子控制器继承父控制器中的内容
 
2，基于事件的方式。即$on,$emit,$boardcast这三种方式
 
3，服务方式。写一个服务的单例然后通过注入来使用


`AngularJS控制器controller如何通信？ - SegmentFault` 
http://segmentfault.com/a/1190000000639592


`学习 ui-router - 管理状态 | bubkoo`
http://bubkoo.com/2014/01/01/angular/ui-router/guide/state-manager/


`UI Router API`
http://angular-ui.github.io/ui-router/site/#/api/ui.router.state.directive:ui-sref


