title: chrome Developer：Clear Timer
date: 2015-10-06 00:00:00
tags: [chrome]
 
---


#chrome Developer  技巧 Event Listener Breakpoints 之 Timer - Clear Timer
---
目标：代码控制浏览器断点停留，调试内存区javaScript



##适用场景：
<pre>
1.  内存中 【VM****】运行的Script代码，因为无法搜索到，仅能在内存运算过程中监听到。
2. 所有无法手动打断点的场景。  
</pre>



###操作：
<pre>
1.设置 Event Listener Breakpoints的监听 Timer - Clear Timer


2.代码，触发点
clearTimeout(setTimeout( "" )); // web debug   


3.代码，断点
`debugger;`

`debug(functoinName)`
</pre>



![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/81202651.jpg)



## 事件断点：脚本声明（`sript First statement`） `适合使用了` `use Strict`声明的js文件
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/92314456.jpg)





### 更多方法
```
debugger;
```
```
debug(funcName);
```
详见: `javaStript Tips.md`