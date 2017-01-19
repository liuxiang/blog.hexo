title: 监听video播放事件 tips
date: 2017-1-7 00:00:00
tags: [video]


---
# 监听timeupdate事件，在触发后，立即关掉监听。即为播放事件
```
var vid = document.getElementById("myVideo");//  var vid =   document.querySelector('video');
vid.addEventListener('timeupdate', function timeupdateListener() {
  document.getElementById("demo2").innerHTML = '开始播放:' + vid.currentTime;// 
  vid.removeEventListener("timeupdate", timeupdateListener);// 删除当前事件监听
});
```


`测试代码`
http://www.runoob.com/try/try.php?filename=tryhtml5_av_event_timeupdate
