title:  ionic嵌套路由


date: 2017-5-8 00:00:00
tags: [ionic ]


---


# getting-started
http://ionicframework.com/getting-started
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/30893720.jpg)


---
# 示例： ionic-starter-tabs
https://github.com/driftyco/ionic-starter-tabs


http://plnkr.co/edit/qYMCrt?p=preview
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/36107114.jpg)


- tab / tab.dash， tab.friends
```
  $stateProvider


    // setup an abstract state for the tabs directive
    .state('tab', {
      url: "/tab",
      abstract: true,
      templateUrl: "tabs.html"
    })


    // Each tab has its own nav history stack:


    .state('tab.dash', {
      url: '/dash',
      views: {
        'tab-dash': {
          templateUrl: 'tab-dash.html',
          controller: 'DashCtrl'
        }
      }
    })


    .state('tab.friends', {
      url: '/friends',
      views: {
        'tab-friends': {
          templateUrl: 'tab-friends.html',
          controller: 'FriendsCtrl'
        }
      }
    })
    .state('tab.friend-detail', {
      url: '/friend/:friendId',
      views: {
        'tab-friends': {
          templateUrl: 'friend-detail.html',
          controller: 'FriendDetailCtrl'
        }
      }
    })


    .state('tab.account', {
      url: '/account',
      views: {
        'tab-account': {
          templateUrl: 'tab-account.html',
          controller: 'AccountCtrl'
        }
      }
    })


  // if none of the above states are matched, use this as the fallback
  $urlRouterProvider.otherwise('/tab/dash');
```


- tabs.html
```
<ion-tabs class="tabs-icon-top">




  <!-- Pets Tab -->
  <ion-tab title="Dashboard" icon="icon ion-home" href="#/tab/dash">
    <ion-nav-view name="tab-dash"></ion-nav-view>
  </ion-tab>




  <!-- Adopt Tab -->
  <ion-tab title="Friends" icon="icon ion-heart" href="#/tab/friends">
    <ion-nav-view name="tab-friends"></ion-nav-view>
  </ion-tab>




  <!-- About Tab -->
  <ion-tab title="Account" icon="icon ion-gear-b" href="#/tab/account">
    <ion-nav-view name="tab-account"></ion-nav-view>
  </ion-tab>


</ion-tabs>
```


---

# 示例： ionic-starter-sidemenu
https://github.com/driftyco/ionic-starter-sidemenu


http://plnkr.co/edit/0RXSDB?p=preview
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/68685846.jpg)


# 侧边栏+菜单
-  app
```
$stateProvider
  .state('app', {
    url: '/app',
    abstract: true,
    templateUrl: 'app/app/menu.html',
    controller: 'AppCtrl'
  });
```
-  app.table
```
$stateProvider
  .state('app.table', {
    url: '/table',
    templateUrl: 'app/table/table.html',
    controller: 'TableCtrl'
    // views:{
    //   'tableContent':{
    //     templateUrl:'app/table/table.html',
    //     controller:'TableCtrl'
    //   }
    // }
  })
```
-  app/table/table.html （`自定义嵌套路由页面`/` <ion-tabs` ）
```
<ion-view view-title="" class="table_page" hide-nav-bar="true">


  <ion-content class="table_content" overflow-scroll="true">
    <div class="table_panel">


      <div class="table_div">


        <!--主页-->
        <button class="button buttons button-clear header-item ws_tab" ng-click="forward(4)">
          <!--<i class="icon ion-social-youtube-outline" style="color: white;"></i>-->
          <div>
            <img src="img/table/nav/6.png" class="tab_img" ng-if="active == 4">
            <img src="img/table/nav/5.png" class="tab_img" ng-if="active != 4">
          </div>
          <div class="tab_text" ng-style="{true:{color:'#56b0ec'}}[active == 4]">主页</div>
        </button>


        <!--课件-->
        <button class="button buttons button-clear header-item ws_tab" ng-click="forward(1)">
          <!--<i class="icon ion-android-home" style="color: white;"></i>-->
          <div>
            <img src="img/table/nav/4.png" class="tab_img" ng-if="active == 1">
            <img src="img/table/nav/3.png" class="tab_img" ng-if="active != 1">
          </div>
          <div class="tab_text" ng-style="{true:{color:'#56b0ec'}}[active == 1]">课件</div>
        </button>


        <!--专项-->
        <button class="button buttons button-clear header-item ws_tab" ng-click="forward(2)" ng-if="$root.ios_isPayChoose">
          <!--<i class="icon ion-ios-bookmarks-outline" style="color: white;"></i>-->
          <div>
            <img src="img/table/nav/2.png" class="tab_img" ng-if="active == 2">
            <img src="img/table/nav/1.png" class="tab_img" ng-if="active != 2">
          </div>
          <div class="tab_text" ng-style="{true:{color:'#56b0ec'}}[active == 2]">专项</div>
        </button>


        <!--<button ui-sref="app.table.live" class="button buttons button-clear header-item" ng-click="active=3">-->
          <!--&lt;!&ndash;<i class="icon ion-social-youtube-outline" style="color: white;"></i>&ndash;&gt;-->
          <!--<img src="img/table/nav/play_active.png" style="height: 30px;" ng-if="active == 3">-->
          <!--<img src="img/table/nav/play.png" style="height: 30px;" ng-if="active != 3">-->
        <!--</button>-->
      </div>
    </div>


    <div ng-show="active == 4">
      <ion-nav-view name="mainContent"></ion-nav-view>
    </div>
    <div ng-show="!active || active == 1">
      <ion-nav-view name="homeContent"></ion-nav-view>
    </div>
    <div ng-show="active == 2">
    <ion-nav-view name="specialContent"></ion-nav-view>
    </div>
    <!--<div ng-show="active == 3">-->
      <!--<ion-nav-view name="liveContent"></ion-nav-view>-->
    <!--</div>-->
  </ion-content>
</ion-view>
```


-  app.table. main
```
$stateProvider
  .state('app.table.main', {
    url: '/main',
    // cache:false,
    views: {
      'mainContent': {
        templateUrl: 'app/main/main.html',
        controller: 'MainCtrl'
      }
    }
  })
```


---
# 使用`ion-tabs`  存在二级路由丢失返回键问题
```
<!--<ion-view view-title="table" class="page-tab">-->
<!--<ion-view class="page-tab"> &lt;!&ndash; 取消路由页面的view-title避免子类复写失败 &ndash;&gt;-->


<div class="page-tab">
  <!-- 注意：存在二级路由丢失返回键问题 -->
  <!--<ion-tabs class="tabs-icon-top tabs-color-active-positive my-tab">-->
  <!--&lt;!&ndash; Dashboard Tab &ndash;&gt;-->
  <!--<ion-tab title="首页" icon-off="ion-ios-home-outline" icon-on="ion-ios-home" href="#/app/tab/home">-->
  <!--<ion-nav-view name="tab-home"></ion-nav-view>-->
  <!--</ion-tab>-->


  <!--&lt;!&ndash; Chats Tab &ndash;&gt;-->
  <!--<ion-tab title="设备" icon-off="ion-ios-chatboxes-outline" icon-on="ion-ios-chatboxes" href="#/app/tab/device">-->
  <!--<ion-nav-view name="tab-device"></ion-nav-view>-->
  <!--</ion-tab>-->


  <!--&lt;!&ndash; Account Tab &ndash;&gt;-->
  <!--<ion-tab title="工单" icon-off="ion-ios-list-outline" icon-on="ion-ios-list" href="#/app/tab/work">-->
  <!--<ion-nav-view name="tab-work"></ion-nav-view>-->
  <!--</ion-tab>-->


  <!--<ion-tab title="个人" icon-off="ion-ios-person-outline" icon-on="ion-ios-person" href="#/app/tab/account">-->
  <!--<ion-nav-view name="tab-account"></ion-nav-view>-->
  <!--</ion-tab>-->
  <!--</ion-tabs>-->


  <!-- ------------------------------------------------------------ -->


  <!-- 注意：自定义路由,可解决二级路由丢失返回键问题 -->
  <div ng-show="isShow('app.tab.home')">
    <ion-nav-view name="tab-home"></ion-nav-view>
  </div>
  <div ng-show="isShow('app.tab.device')">
    <ion-nav-view name="tab-device"></ion-nav-view>
  </div>
  <div ng-show="isShow('app.tab.work')">
    <ion-nav-view name="tab-work"></ion-nav-view>
  </div>
  <div ng-show="isShow('app.tab.account')">
    <ion-nav-view name="tab-account"></ion-nav-view>
  </div>


  <div class="table_button">
    <!--<div class="" ui-sref="app.tab.home">-->
    <div class="" ng-click="go('app.tab.home')">
      <img ng-src="img/table/nav/{{isShow('app.tab.home')?2:1}}.png" class="tab_img">
      <div>首页</div>
    </div>
    <!--<div class="" ui-sref="app.tab.device">-->
    <div class="" ng-click="go('app.tab.device')">
      <img ng-src="img/table/nav/{{isShow('app.tab.device')?4:3}}.png" class="tab_img">
      <div>设备</div>
    </div>
    <!--<div class="" ui-sref="app.tab.work">-->
    <div class="" ng-click="go('app.tab.work')">
      <img ng-src="img/table/nav/{{isShow('app.tab.work')?6:5}}.png" class="tab_img">
      <div>工单</div>
    </div>
    <!--<div class="" ui-sref="app.tab.account">-->
    <div class="" ng-click="go('app.tab.account')">
      <img ng-src="img/table/nav/{{isShow('app.tab.account')?6:5}}.png" class="tab_img">
      <div>个人</div>
    </div>
  </div>
</div>
<!--</ion-view>-->
```
```
{
  $scope.isShow = isShow;
  $scope.go = function (value) {
    // $ionicHistory.nextViewOptions({historyRoot: true});// 路由框架页面,全部升级为顶层路由(不同于默认顶层root)
    $state.go(value);
  }
}


function isShow(stateName) {
  return stateName == $state.current.name;
}
```


---
#  取消路由页面的view-title避免子类复写失败
```
<!--<ion-view view-title="table" class="page-tab">-->
<!--<ion-view class="page-tab"> &lt;!&ndash; 取消路由页面的view-title避免子类复写失败 &ndash;&gt;-->
```


---
# tab切换出现菜单
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/9520430.jpg)



-  出现情况，index=1 复用了historyId
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/38952069.jpg)

 
- 不出现情况，各自为index=0
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/35408752.jpg)


- 问题原因：嵌套路由间切换，同样会发生历史轨迹，出现返回键的情况
- 解决办法：对嵌套路由间切换，全部升级为顶层路由 (不同于默认顶层root)
```
$scope.go = function (value) {
  $ionicHistory.nextViewOptions({historyRoot: true});// 路由框架页面,全部升级为顶层路由(不同于默认顶层root)
  $state.go(value);
}
```


---
# 同时使用 Tabs + Side Menu
` Ionic Tabs + Side Menu `
http://loring-dodge.azurewebsites.net/ionic-tabs-plus-side-menu/
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/22491562.jpg)

 
---
**参考**
`ionic back 返回按钮不正常显示&&二级路由点击返回按钮失效无法返回到上一级页面的问题`

http://blog.csdn.net/badgirl_hong/article/details/53505919


`ionic tab(选项卡) | 菜鸟教程`
http://www.runoob.com/ionic/ionic-tab.html

