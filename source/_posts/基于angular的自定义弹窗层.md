title: 基于angular的自定义弹窗层
date: 2016-08-30 00:00:01
tags: [ angular的自定义弹窗层]


---
# angular 自定义弹窗层
- *controller.js

```
        /**
         * 弹出公共的金币奖励提示
         */
        $http.get('app_module/gold/gold.html', ({cache: $templateCache} || {})).then(function (response) {
          // 1.获取 template 标签(也是新建)
          var goldEL = angular.element(response.data);
 
          // 1.2 更新金币数量
          $scope.number = Math.floor(Math.random() * 5) + 1;
          $compile(goldEL)($scope);// $scope双向绑定
 
          // 2.动态添加template 标签
          angular.element(document).find('body').append(goldEL);
 
          // 3.3s后自动删除此标签
          $timeout(function () {
            goldEL.remove();
          }, 1500);
        });
```
- app_module/gold/gold.html
```
<style>
  .gold_panel {
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    /*background-color: red;*/
    z-index: 999;
  }
 
  .gold_panel > img {
    width: 50vw;
    height: 50vw;
  }
</style>
 
<div class="gold_panel">
  <img class="_slideDown" src="img/transcript/level-{{number}}.png">
</div>
```


- angular模块化封装
AutoDiv.md



---

# ionic弹窗层可基于$ionicPopup重写
```
$scope.showPopup = function() {
    var myPopup = $ionicPopup.show({
        cssClass:'er-popup',
        templateUrl: 'templates/er_modal.html',
        scope: $scope
    });
    myPopup.then(function(res) {


    });
};
```
`ionic使用记录----popup - mainpro126的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/mainpro126/article/details/46342157