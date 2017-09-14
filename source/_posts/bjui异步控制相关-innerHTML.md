title: bjui异步控制相关- innerHTML  
date: 2017-4-7 00:00:00
tags: [ innerHTML ]


---


# 查询，刷新当前窗口（navtab）
- bjui-ajax.js `Plugin.call($form, 'doSearch', options)`开始
```
$form.validator({
    valid: function(form) {
        Plugin.call($form, 'doSearch', options)
    }
})
```


- bjui-extends.js `$this.empty().html(response)`
```
$.ajax({
    type: op.type || 'GET',
    url: op.url,
    data: op.data || {},
    cache: false,
    dataType: 'html',
    timeout: BJUI.ajaxTimeout,
    success: function(response) {        $this.empty().html(response).append($ajaxMask).initui();// 清空目标文档+写入新的html+重新初始化ui（可执行script）
    }
    ...

```


`让 innerHTML 进来的 script 代码跑起来`

http://www.cnblogs.com/zichi/p/run-innerHTML-script.html



- bjui-initui.js (各状态事件推送)
```
Initui.prototype.init = function() {
    var that = this, $element = that.$element


    $.when(that.initUI()).done(function(){
        $element.trigger(BJUI.eventType.afterInitUI)
    })
}
```


---
## 分析方法：
选择被刷新的标签，设置节点的删除监听`break on - Node removal`.断点后，查看线程堆栈分析过程
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/55723545.jpg)
 