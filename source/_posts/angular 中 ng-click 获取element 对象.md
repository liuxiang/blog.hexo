title: angular 中 ng-click 获取element 对象
date: 2017-1-4 00:00:00
tags: [angular]


---
```
<ul id="team-filters">
    <li ng-click="foo($event, team)" ng-repeat="team in teams">
         <img src="{{team.logoSmall}}" alt="{{team.name}}" title="{{team.name}}">
    </li>
</ul>
```
```
$scope.foo = function($event, team) {
   var el = (function(){
       if ($event.target.nodeName === 'IMG') {
          return angular.element($event.target).parent(); // get li
       } else {
          return angular.element($event.target);          // is li
       }
   })();
```
`get original element from ng-click`

http://stackoverflow.com/questions/23107613/get-original-element-from-ng-click
