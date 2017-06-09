title: angular图片预览
date: 2017-4-7 00:00:02
tags: [ 图片预览 ]


---


- html实现
```
<div class="form-group">
  <label for="InputFile" class="col-sm-3 control-label">图片</label>
  <div class="col-sm-9">
    <input id="InputFile" type="file"
           onchange="angular.element(this).scope().modalData.icon = window.URL.createObjectURL(this.files[0]);
                       angular.element(this).scope().$apply()"/>
    <img ng-src="{{modalData.icon}}">
  </div>
</div>
```


- 指令实现
```
<div class="form-group">
  <label for="layout_img" class="col-sm-3 control-label">图片{{modalData.layer.background}}</label>
  <div class="col-sm-9">
    
    <!--<input id="layout_img" type="file" fileread="uploadme" ng-model="modalData.layer.background"/>-->
    <!--<img ng-src="{{modalData.layer.background}}">-->
    <input id="layout_img" type="file" fileread="modalData.layer.background"/>
    
    <img class="layout-img" ng-src="{{modalData.layer.background.url}}">
  </div>
</div>
```
```
.directive("fileread", [function () {
  return {
    scope: {
      fileread: "="
    },
    link: function (scope, element, attributes) {
      element.bind("change", function (changeEvent) {
        scope.$apply(function () {
                scope.fileread = changeEvent.target.files[0];
              scope.fileread.url = URL.createObjectURL(changeEvent.target.files[0]);        
      });
    }
  }
}])
```