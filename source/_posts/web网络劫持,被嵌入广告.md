title: web网络劫持,被嵌入广告
date: 2016-09-20 00:00:00
tags: [网络劫持, 广告 ]
 
---
# 常见源头:

1.是不是我们项目中的第三方jar非官方版本，被植入了一些广告代码   (★)
2.dns劫持  (★★★)
3.http劫持 (★★★★)
 
# 排查:
检查DNS,替换安全DNS观察广告是否存在
启用使用https服务,观察广告是否存在
 
# 防护办法:
>替换安全DNS
>使用https服务
>nginx出口网络过滤
>使用第三方官方工具
>ios/android webView过滤
>前端页面javaScript主动防御


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/55107594.jpg)


`让那些烦人的广告，滚出我们的APP！ - 吴焱的博客 - 博客频道 - CSDN.NET` 
http://blog.csdn.net/wy1002003/article/details/51783623


` 联通宽带劫持网页，JS 弹窗广告，该如何解决？ - 知乎`
https://www.zhihu.com/question/23197534
