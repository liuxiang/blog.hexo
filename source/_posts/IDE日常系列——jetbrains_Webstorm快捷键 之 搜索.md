title: IDE日常系列——Webstorm 之 搜索
date: 2016-1-9 00:00:00 #发表日期，一般不改动
categories:  Webstorm  #文章文类
tags: [ Webstorm ]


---


# 万能搜索 Double Shift (双击Shift键)[ Shift+Shift ]
> 且会展示各类资源的快捷键


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/95893666.jpg)


# N
[Ctrl+N]                        Classes 查找类对象,js对象
[ Ctrl+Alt+N ]           未知(存在wiz创建笔记占用可能)

[Ctrl +Shift+N]           Files 查找文件 
[Ctrl+Alt+Shift+N]   Symbols 符号搜索,属性搜索 

# B
[ Ctrl+B]                声明 declaration
[ Ctrl+Alt+B ]       实现 implementation ->继承
[Ctrl+Shift+B]   类型声明 [类资源] ( 引用? )   ★ ★
> 结果类似 Ctrl+Alt+Shift+N  符号搜索,属性搜索[ 文件 资源]. 但结果排版对齐有区别.范围也不同.





[Ctrl+Alt+Shift+B ]     无功能


# F
[Ctrl+F]               当前类, 输入搜索
[Ctrl+Shift+F]   全局配置搜索
> 特色支持preview(预览) -v11版新增


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/74796693.jpg)


# F7
[Alt+F7]                列表展开,对象使用位置

[Ctrl+F7]               快速选择到下一个引用处[当前方法- 对象引用 ](参数)  ★

[Ctrl+Alt+F7]       对象引用  ★
[Ctrl+Shift+F7]    选择当前类此字符串. 同 Ctrl+F (建议结合Ctrl+L/Ctrl+ShiftL 选择上一个,下一个)  同`F3`,`Shift+F3`
[Ctrl+Alt+Shift+F7] 同  [Ctrl+Shift+F7]



# 选中单词 上下文搜索
选中>Ctrl+F
Ctrl+L 下一个
Ctrl+shift+L 上一个


# eclipse特色功能
`搜索`  出现在文件中对象应用  Ctrl+Shift+U     ws:Alt+F7 (设置为范围为当前打开文件)

或者 `Ctrl+F`(搜索) + `Ctrl+L`(下一个)



- 清理不需要的展示
 


`搜索` 文件中方法   Ctrl+O                     ws:Alt+7


<!-- more -->


