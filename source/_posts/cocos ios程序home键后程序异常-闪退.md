title: cocos ios程序home键后程序异常/闪退
date: 2016-09-23 00:00:00
tags: [ cocos ]


---
# home键后程序异常日志
```
libGPUSupportMercury.dylib`gpus_ReturnNotPermittedKillClient:
    0x2f3df76c <+0>:  movw   r0, #0xbeef
    0x2f3df770 <+4>:  movs   r1, #0x1
    0x2f3df772 <+6>:  movt   r0, #0xdead
->  0x2f3df776 <+10>: str    r0, [r1]
    0x2f3df778 <+12>: bx     lr
    0x2f3df77a <+14>: nop   
 
webThread(9):exc_bad_access(code=1,address=0x1)
```


---
director在app切换至后台时若被pause，它其实并没有真正暂停，而是仍保持着每秒4帧的绘制速率。但在app处于后台时，ios是不允许app调opengl的。这个原因引发了上述问题。
 
- 解决方法：
```
[cpp] view plain copy
void AppDelegate::applicationDidEnterBackground() 
{ 
    CCDirector::sharedDirector()->stopAnimation(); 
} 
void AppDelegate::applicationWillEnterForeground() 
{ 
    CCDirector::sharedDirector()->startAnimation(); 
} 
``` 
`按Home键切换到后台后会触发libGPUSupportMercury.dylib: gpus_ReturnNotPermittedKillClient导致crash - Mr_Ray的IT专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/world_liu/article/details/8960786 


---
# 原因分析
在cocos2d-x 2.0.4版本上，当点击HOME键将程序切换后台，有时会导致crash，程序停在在
gpus_ReturnNotPermittedKillClient函数，搜了一下此问题原因，有几篇文章比较好，
 
http://blog.k-res.net/archives/1193.html，这片文章说了此问题的原因，以及其在项目中的解决方案。
 
Implementing a Multitasking-aware OpenGL ES Application，这个是苹果对于此问题的解释，以及部分解决方案。
 
以上文章对于此问题做了比较详细的分析与解释，虽然不能解决游戏中此问题，但是对于了解问题本质很有帮助。
 
对于cocos2d-x中解决vi问题，
```
在AppDelegate::applicationDidEnterBackground()中增加CCDirector::sharedDirector()->stopAnimation()；
在AppDelegate::applicationWillEnterForeground()中增加CCDirector::sharedDirector()->resume()。
```


原因是在applicationDidEnterBackground函数中原来调用了CCDirector::sharedDirector()->pause()函数，此函数并没有真正停止动画，而是以1/4秒在继续运行，这样就会导致程序在后台渲染，这是apple不允许的，所以导致crash。


`cocos2d-x 2.0.4按home键切换后台导致gpus_ReturnNotPermittedKillClient crash_游戏症候群_新浪博客`
http://blog.sina.com.cn/s/blog_62b2318d0101lc0n.html


`官网描述`
https://developer.apple.com/library/content/qa/qa1766/_index.html


