title: ionic 路由view综合控制
date: 2016-08-27 00:00:00
tags: [ionic路由控制]


---
官网  http://angular-ui.github.io/ui-router/
API http://angular-ui.github.io/ui-router/site/
Quick Reference( wiki ) https://github.com/angular-ui/ui-router/wiki/Quick-Reference


---
# ionic 禁止侧滑后退
```
//禁止侧滑后退事件
$ionicConfigProvider.views.swipeBackEnabled(false);
```


# ionic中路由返回控制 ```
 $ionicHistory.goBack();              //  ionic控制历史
 $ionicHistory.goBack(-2); 


$ionicHistory.viewHistory().backView.url  // 预测当前页,ionic返回的目标页面

$ionicHistory.viewHistory().views // 已记录的路由

$ionicHistory.currentHistoryId(); // 历史堆栈的 ID，是当前视图的父类容器



window.history.back();                    // html控制历史
```
`$ionicGoBack()`


`API`  http://ionicframework.com/docs/api/service/$ionicHistory/


# 路由切换的几种方法
```
$location.path("/path/origin")



$state.go('app.section');
https://github.com/angular-ui/ui-router/wiki/Quick-Reference#stategoto--toparams--options


ui-sref="tab.exp({bagId:books[$index].id}) // 通过路由参数传递数据
ui-sref="tab.exp({bagId:{{books[$index].id}}}) // 通过路由参数传递json数据
注: 不建议:数据会显示在地址栏
https://github.com/angular-ui/ui-router/wiki/Quick-Reference#ui-sref


// identical to previous example
url: "/contacts/{contactId}" 
https://github.com/angular-ui/ui-router/wiki/URL-Routing



标签 href = '#app/home'
js     window.location.href='#app/home' 
注:不建议:会重新加载应用
```


# 路由间数据传递方法
```
/**
* 路由数据(单例)
*/
angular.module('starter')
  .factory('RouteData', function () {
    return {}
  })
```

```
RouteData.transcript = {questionRoot: questionRoot};
localStorageService.set('transcript', {questionRoot: questionRoot});// TODO 临时,本地存储
$state.go('app.transcript');
```
```
if (RouteData.transcript == null) {
     RouteData.transcript = localStorageService.get("transcript");
}
var questionRoot = RouteData.transcript.questionRoot;// 接收成绩单
```


# 禁止缓存
- 时间戳，让缓存资源不重复
```
app.config(function($routeProvider, $locationProvider) { 
  $routeProvider 
   .when('/Book/:bookId/ch/', { 
    templateUrl: 'chapter.html' + '?datestamp=' + (new Date()).getTime(), 
    controller: 'ChapterController' 
  }); 
});  
```
- 同步删除 模板缓存 
```
app.run(function($rootScope, $templateCache) { 
    $rootScope.$on('$routeChangeStart', function(event, next, current) { 
        if (typeof(current) !== 'undefined'){ 
            $templateCache.remove(current.templateUrl); 
        } 
    }); 
});  
```
` AngularJs 禁止模板缓存 - 君醉酒的博客 - 博客频道 - CSDN.NET`
http://blog.csdn.net/rain097790/article/details/50570041



- ionic 禁用缓存
```
$ionicConfigProvider.views.maxCache(0);

```
- 单路由，取消缓存
```
$stateProvider.state('myState', {
   cache: false,
   url : '/myUrl',
   templateUrl : 'my-template.html'
})
```
```
<ion-view cache-view="false" view-title="My Title!">
  ...
</ion-view>
```


---
# ionic路由相关,综合文章
`ionic js 导航ion-nav-viewionic ionic能够检测到浏览历史,通过检测浏览历史，实现向左或向右滑动时可以正确转换视图。 - yongbin668的博客 - 博客频道 - CSDN.NET`
http://blog.csdn.net/yongbin668/article/details/51601326


---

#  提前为需要返回的目标路由 view设置参数
```
$ionicHistory.backView().stateParams = {card:true};

$ionicViewSwitcher.nextDirection('back'); // 控制着动画( animation )方向
```
https://forum.ionicframework.com/t/change-variable-of-previous-view-before-going-back-with-ionichistory-goback/16309/4


#  ★ 展示 ionic 路由历史列表
```
$scope.$on('$ionicView.enter', function(e) {
        var history = $ionicHistory.viewHistory();
        angular.forEach(history.views, function(view, index){
            console.log('views: ' + view.stateName);
        });
        angular.forEach(history.histories[$ionicHistory.currentHistoryId()].stack, function(view, index){
            console.log('history stack:' + view.stateName);
        });
    });
```
`javascript - $ionicHistory.backView has incorrect state when go to previous state manually - Stack Overflow`

http://stackoverflow.com/questions/31364578/ionichistory-backview-has-incorrect-state-when-go-to-previous-state-manually


#  ★ 提前设置ionic 路由 返回目标 
```
$scope.$on('$ionicView.enter', function (e) {
    var i,
        currentHistoryId = $ionicHistory.currentHistoryId(),
        history = $ionicHistory.viewHistory(),
        stack = history.histories[currentHistoryId].stack;
 
    for (i = 0; i < stack.length; i += 1) {
        if (stack[i].stateId.indexOf("app.audit_idAudit=") == 0) {
            $ionicHistory.backView(stack[i]);
            break;
        }
    }
});
```
`javascript - $ionicHistory set backView(view) but back button title is not being updated - Stack Overflow`
http://stackoverflow.com/questions/34789813/ionichistory-set-backviewview-but-back-button-title-is-not-being-updated


---


# 在路由存在缓存的情况下,实现路由改变后的数据更新
```
$scope.$on('$stateChangeSuccess', $scope.rapidResponse.update); // 如果有变化即重新调用获取数据函数，实现数据更新刷新

```
`使用angularjs、ionic框架如何实现返回上一页并刷新 - 推酷`
http://www.tuicool.com/articles/InQz2iB


# 自定义点击动作(返回)，用 $ionicNavBarDelegate
```
$ionicNavBarDelegate.back();

$ionicNavBarDelegate.getPreviousTitle(); // 获取上一步路由标题

$ionicNavBarDelegate.title(title);// 设置路由标题

```
`ionic js 导航ion-nav-viewionic ionic能够检测到浏览历史,通过检测浏览历史...`
http://blog.csdn.net/yongbin668/article/details/51601326




# 控制view切换动画 ( animation )
```
<ion-nav-view name="menuContent" animation="slide-left-right"></ion-nav-view>

```
```
$ionicModal.fromTemplateUrl('login.html', {
        scope: $scope,
        animation: 'slide-in-up'
    }).then(function (modal) {
            $scope.modal = modal;
    });
```
```
$state.go( '.', { page: nextPage }, { animation: 'no-animation' })

```
`Control the animation in $state.go - ionic - Ionic`
https://forum.ionicframework.com/t/control-the-animation-in-state-go/14492
http://angular-ui.github.io/ui-router/site/#/api/ui.router.state.$state



`javascript - ionic animation失效 - SegmentFault`
https://segmentfault.com/q/1010000005668784



##  Ionic 取消自带（默认）动画效果
```
$ionicConfigProvider.views.transition('none');
或：
<ion-view view-title="个人中心" animation="no-animation"> 
```


` Ionic 取消自带动画效果 - jack088的个人页面 - 开源中国社区 `
https://my.oschina.net/jack088/blog/543006



- angular动画使用
`AngularJS 应用中实现 JavaScript 动画效果 - 技术翻译 - 开源中国社区`
http://www.oschina.net/translate/javascript-animations-angularjs-applications
 
`AngularJS应用页面切换优化方案 - 葡萄城控件技术团队 - 博客园`
http://www.cnblogs.com/powertoolsteam/p/Wijmo_XProject_1.html


---
# 下一个路由页面控制
## $ionicHistory.nextViewOptions()
Sets options for the next view. This method can be useful to override certain view/transition defaults right before a view transition happens. For example, the menuClose directive uses this method internally to ensure an animated view transition does not happen when a side menu is open, and also sets the next view as the root of its history stack. After the transition these options are set back to null.
 
Available options:
 
- disableAnimate: Do not animate the next transition.
- disableBack: The next view should forget its back view, and set it to null.
- historyRoot: The next view should become the root view in its history stack.  (设置下一个页面为根页面)
```
$ionicHistory.nextViewOptions({
  disableAnimate: true,
  disableBack: true
});
```


---
# 忠告
## 尽量不要使用` $state.go` 来实现返回动作,因为会与$ionicHistory的路由历史控制脱轨.
- 影响: 
1.下一步的再返回,可能会放回到上一个` $state.go` 的页面,而不能按照原\$ionicHistory轨迹 返回
2.android用户如果使用手机的返回键,会触发\ $ionicHistory路由历史轨迹,不能执行到 ` $state.go`的代码
2.1 为弥补问题,就需要重写返回键来控制.(时间久了产生不同步,会出现此容易忽视的返回问题)


- 建议的处理方法, 参考上文中各场景处理示例
    -  路由间数据传递方法

    - 提前为需要返回的目标路由view设置参数

    -  ★展示ionic路由历史列表
    -  ★提前设置ionic路由返回目标 
    -  在路由存在缓存的情况下,实现路由改变后的数据更新


---
# 问题

## 在次页面主动跳转到主页面,ionicHistory会存在历史,导致首页无法显示菜单图标
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/58277758.jpg)
`ionic导航之后返回功能的说明 - 阿锋佬 - 博客园`
http://www.cnblogs.com/feng18/p/5217680.html


- 原因
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/87393479.jpg)
 
`viewData.enableBack` 存在了历史


---


```
return {
        viewId: viewId,
        action: action,
        direction: direction,
        historyId: historyId,
        enableBack: this.enabledBack(viewHistory.currentView),
        isHistoryRoot: (viewHistory.currentView.index === 0),
        ele: ele
      };
```
```
enabledBack: function(view) {
      var backView = getBackView(view);
      return !!(backView && backView.historyId === view.historyId); # 同一主路由,backView不为空,则存在,返回true
    },
```
```
  function getBackView(view) {
    return (view ? getViewById(view.backViewId) : null);
  }
```
```
  var viewHistory = {
    histories: { root: { historyId: 'root', parentHistoryId: null, stack: [], cursor: -1 } },
    views: {},
    backView: null,
    forwardView: null,
    currentView: null
  };
```
- 最后属性赋值
```
        transition: function(direction, enableBack, allowAnimate) {
          var deferred;
          var enteringData = getTransitionData(viewLocals, enteringEle, direction, enteringView);
          var leavingData = extend(extend({}, enteringData), getViewData(leavingView));
          enteringData.transitionId = leavingData.transitionId = transitionId;
          enteringData.fromCache = !!alreadyInDom;
          enteringData.enableBack = !!enableBack;
          enteringData.renderStart = renderStart;
          enteringData.renderEnd = renderEnd;
```




---
- B页面检查
```
$ionicHistory.viewHistory().currentView.historyId
$ionicHistory.getViewById($ionicHistory.viewHistory().currentView.backViewId).historyId
```


- 我的解决办法
通过路由回到根目录,避免首页出现返回图标

```
$ionicHistory.goToHistoryRoot($ionicHistory.currentHistoryId());// 通过路由回到根目录避免首页出现返回图标

```
- 方法二
```
$ionicHistory.nextViewOptions({historyRoot: true});// 设置下一个页面为根页面

```