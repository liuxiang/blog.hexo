title: angular ng-click程序触发
date: 2016-09-13 00:00:00
tags: [angular]
 
---
# angular ng-click程序触发,方法
```
document.querySelector('.nav-bar-title.ng-binding').click();// no
$('.nav-bar-title.ng-binding').click();// no
$('.nav-bar-title.ng-binding').trigger('click',function(){});// no
$('.nav-bar-title.ng-binding').triggerHandler('click'); // no


angular.element( document.querySelector('.nav-bar-title.ng-binding') ).triggerHandler('click');// ok
angular.element($('.nav-bar-title.ng-binding')).triggerHandler('click');// ok

```


# 差异
triggerHandler() 方法与 trigger() 方法类似。不同的是它不会触发事件（比如表单提交）的默认行为，而且只影响第一个匹配元素。事件不会在 DOM 树中冒泡

```
bind()    向元素添加事件处理程序

unbind()    从被选元素上移除添加的事件处理程序



on()    向元素添加事件处理程序

off()    移除通过 on() 方法添加的事件处理程序


click()    添加/触发 click 事件

trigger()    触发绑定到被选元素的所有事件
triggerHandler()    触发绑定到被选元素的指定事件上的所有函数


event.stopPropagation()    阻止事件向上冒泡到 DOM 树，阻止任何父处理程序被事件通知

```
http://www.runoob.com/jquery/jquery-ref-events.html


---
# 选择器比较
```
console.dir(document.querySelector('.sections'))
VM8305:1 div.sections
console.dir(document.querySelector('.sections:nth-child(1)'))
VM8432:1 div.sections


console.dir(document.querySelectorAll('.sections'))
VM8433:1 NodeList[38]
console.dir(document.querySelectorAll('.sections')[1])
VM8434:1 div.sections


console.dir($('.sections'))
VM8306:1 init[38]
console.dir($('.sections)')[0])
VM8428:1 div.sections
```


---
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/86121546.jpg)
 



