title: 正则格式化图书目录 2 xmind 之 Tab区别
date: 2016-2-1 00:00:00 #发表日期，一般不改动
categories: 正则   #文章文类
tags: [ 正则 ,xmind,书录 ]


---


# Tab的区别
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/41625809.jpg)




# Tab有什么用
4个空格. 粘贴到xmind,不能有效形成目录, 如
![](http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/45208894.jpg)


单个tab字符. 粘贴到xmind,有效形成目录,如
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/15305404.jpg)


# Tab场景
## 4个空格:
* sublime手动tab(修改过Settings User)
*   sublime内容区拷贝' 单个tab字符 ',粘贴 ,会变成4个空格.
*   win记事本,手动tab->复制粘贴到sublime内容区,会变成4个空格.



## 单个tab字符:
* sublime手动tab(未修改过Settings User,默认值为tab字符)

*   win记事本,手动tab
*   win记事本,手动tab->复制粘贴到sublime待替换区( Ctrl+H),依然是tab字符.



---


# sublime text修改TAB缩进为空格(撤销配置,即可还原默认Tab为单字符)


在sublime text中将TAB缩进直接转化为4个空格，可以按照如下方式操作：  
菜单栏: Preferences -> Settings – More -> Syntax Specific – User
  然后添加设置代码就可以了，文件保存在$Packages/User下

```
"tab_size": 4,
"translate_tabs_to_spaces": true
```


<!-- more -->


