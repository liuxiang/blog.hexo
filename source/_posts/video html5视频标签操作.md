title: video html5视频标签操作
date: 2016-10-21 00:00:02
tags: [ video ]



---
- 可运行示例
```
<div class="dv" style="position: relative;">
  <video controls="controls" preload="none" style="width: 100%;" type="video/mp4"
         src="http://cloud.video.taobao.com/play/u/2554695624/p/1/e/6/t/1/fv/102/28552077.mp4"
  >
    <!--poster="http://www.w3school.com.cn/i/w3school_logo_black.gif"-->
    <p>您的浏览器不支持 video 标签</p>
  </video>
  <div style="position: absolute;bottom: 50px;" >hello</div>
</div>
```
---


`js html5 video标签，怎么捕捉到视频最大化的事件呢？-CSDN论坛-CSDN.NET-中国最大的IT技术社区`

http://bbs.csdn.net/topics/390543495


```
var screen_change_events = "webkitfullscreenchange mozfullscreenchange fullscreenchange MSFullscreenChange";
$(document).on(screen_change_events, function () {
console.log('change',document['webkitIsFullScreen'],screen_change_events);
});
```
`javascript - Detecting Event change in fullscreen mode Internet explorer - Stack Overflow`
http://stackoverflow.com/questions/16069548/detecting-event-change-in-fullscreen-mode-internet-explorer


---
# 前端元素全屏
执行全屏：
```
document.querySelector('video')['webkitRequestFullscreen']()

document.querySelector('.prism-player')['webkitRequestFullscreen']()
document.querySelector('.dv')['webkitRequestFullscreen']()
或
document.querySelector('video').webkitRequestFullScreen()

```
注意：控制台直接执行会提示"API只能由用户手势启动"。如按钮触发是正常的
```
document.querySelector('video').webkitRequestFullScreen()
VM454:1 Failed to execute 'requestFullScreen' on 'Element': API can only be initiated by a user gesture.
```
撤销全屏：`document[requestFullScreen.cancelFn]()`
是否全屏：`document['webkitIsFullScreen']`
 
```
requestFn: "webkitRequestFullscreen",
cancelFn: "webkitExitFullscreen",
eventName: "webkitfullscreenchange",
isFullScreen: "webkitIsFullScreen"
```
 
参考：
`prismplayer/prism.js`


---
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/68509484.jpg)

`HTML5 requestFullScreen&exitFullscreen全屏兼容方案`

https://my.oschina.net/ososchina/blog/349660


`HTML5的Video标签的属性,方法和事件汇总 - 简书`
http://www.jianshu.com/p/404d01b8e713


`HTML 音频/视频 | 菜鸟教程` `属性,方法和事件`
http://www.runoob.com/tags/ref-av-dom.html
