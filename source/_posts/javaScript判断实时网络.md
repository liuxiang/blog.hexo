title: javaScript判断实时网络
date: 2016-11-18 00:00:02
tags: [ 判断实时网络 ]



---
#  示例
```

fetch("http://test1.wosaitech.cn/sxb_service/service/sys/versionQuery")
.then(function(response) {
  return response.json();
}).then(function(data) {
  console.log('连接中..',data);
}).catch(function(e) {
  console.log("断网了");
});
```
#  使用
```
/**
* 打开知识点
* @param point
*/
function f_openPoint(point, index) {
  if (window.fetch) {
    fetch(window._config.domain_name + "/sys/versionQuery")
      .then(function (response) {
        return response.json();
      }).then(function (data) {
      f_openPoint_hanlder(point, index);
    }).catch(function (e) {
      console.log("网络连接失败 ,请检查网络");
      $ionicLoading.show({template: "网络连接失败 ,请检查网络"});
    });
  } else {
    f_openPoint_hanlder(point, index);
  }
}
```
#  封装
```
$httpFactory.versionQuery_1802().then(function (res) {
  f_openPoint_hanlder(point, index);
}).catch(function (e) {
  $ionicLoading.show({template: "网络连接失败 ,请检查网络"});
});
```


---
# Polyfill

`传统 Ajax 已死，Fetch 永生 - 会影 - SegmentFault`
https://segmentfault.com/a/1190000003810652


`兼容IE6的fetch polyfill - javascript魔法师 - SegmentFault`
https://segmentfault.com/a/1190000006220369
