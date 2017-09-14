title: ionic问题——Popups无法关闭
date: 2016-1-7 00:00:04 #发表日期，一般不改动
categories: ionic  #文章文类
tags: [ ionic , Popups ]


---

# 问题: ionic在页面跳转前 的Popups 与页面调整后页面的Popups,同时出现会使其Popups无法关闭. 
`确认/取消`回调then可被正常执行. 怀疑Popups对象与UI关系被断开.故判断为组件问题


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/99633147.jpg)


# 官方解决于 v1.2.4  2015.10.16 提交代码
https://github.com/driftyco/ionic/commit/5b39fa34dcaa35108aea37c20a037f5786b00d39


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/42560782.jpg)




# 处理问题
检查引用 index.html    <script src="lib/ionic/js/ionic.bundle.js"></script>
检查版本 ionic.bundle.js 版本 Ionic, v1.1.0

依据处理方法,找到位置,修改代码






# 问题搜索新方法: 到github的"Issues"上搜问题解决办法
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/89527662.jpg)



<!-- more -->
