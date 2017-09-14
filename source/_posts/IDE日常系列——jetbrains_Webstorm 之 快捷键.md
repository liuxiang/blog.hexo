title: IDE日常系列——Webstorm 之 快捷键
date: 2015-10-17 00:00:00
tags: [ IDE, Webstorm, 快捷键 ]




---


# 实时代码提示

File - power save mode
setting-Editor>General>Code Completion



# 手动代码提示
Atl+/      默认为：Alt+Space 被冲突，需要重新设置 ' Code Completion - Basic '
`Cyclic Expand Word` 循环展开文档？







# 搜索
万能搜索 Double Shift (双击Shift键)[ Shift+Shift ],其会展示各类资源的快捷键



Recent Files 最近打开的文件 Ctrl+E
Classes 查找类对象,js对象 Ctrl+N
Files 查找文件 Ctrl +Shift+N 
Symbols     符号搜索,属性搜索 Ctrl+Alt+Shift+N


全文搜索: Ctrl+Shift+F (带预览)




# 查询方法定义.
Ctrl+左键 Choose Declaration


Ctrl+F7/Ctrl+Shift+F7     Find Usages Of




Ctrl+Alt+Shift+N  符号搜索,属性搜索 [文件资源]





[Ctrl+N]  查找类对象,js对象 + [Ctrl+F]搜索方法  ★ ★






[Ctrl+Shift+B] 找类型声明 [类资源]   ★ ★
> 结果类似 Ctrl+Alt+Shift+N  符号搜索,属性搜索[ 文件 资源]. 但结果排版对齐有区别.范围也不同.






# 花括号
Ctrl + { 上花括号,重复则到上一级上花括号
Ctrl + } 下化括号 ,重复则到上一级下花括号


# 格式化代码
快捷键：Ctrl+Alt+L
菜单：Code > Reformat Code


格式化代码：格式化import列表Ctrl+Alt+O，格式化代码Ctrl+Alt+L
运行：Alt+Shift+F10运行程序，Shift+F9启动调试，Ctrl+F2停止。
调试：F7/F8/F9分别对应Step into，Step over，Continue。


# 命令搜索栏
Ctrl+Shift+A



# 行操作
Ctrl+Y删除行、Ctrl+D复制行、Ctrl+</>折叠代码


# 自动按语法选中代码
★ Ctrl+W 以及反向的Ctrl+Shift+W了



# 花括号，闭合跳转
？


# 撤销，前进
Ctrl+Z，Ctrl+Shift+Z


# 操作记录的前后导航是（非默认且常用，可替换）

Ctrl+Alt+Left/Right（←/→）     默认,我修改为了 Alt+Left/Right（←/→）,覆盖了窗口切换
Alt+Left/Right（←/→）   是在打开的代码编辑窗口中左右切换.


# Tab 缩进设置
Ctrl+Shift+A 》Setting

Editor - Code Style - javaScript,html,* - Tab Size


# 查看引用
右键-Find Usages【Alt+F7】


# 打开/ 关闭  左侧目录树
F12/Shift+F12


# 选中单词 上下文搜索
选中>Ctrl+F
Ctrl+L 下一个  
Ctrl+shift+L 上一个


# 选择当前类 对象引用
[Ctrl + B] =  [ Ctrl+Alt+F7] = [Ctrl+鼠标左键]  ★ ★


# 选择当前类 字符串
Ctrl+Shift+F7  同 Ctrl+F (建议结合Ctrl+L/Ctrl+ShiftL 选择上一个,下一个) 
Ctrl+Shift+Alt+J  == 变量名重构(当前全文档) ★


# F7 搜索的应用
Ctrl+Alt+F7       对象引用  ★
Alt+F7                列表展开,对象使用位置

Ctrl+F7               快速选择到下一个引用处[当前方法- 对象引用 ](参数)  ★
Ctrl+Shift+F7    选择当前类此字符串. 同 Ctrl+F (建议结合Ctrl+L/Ctrl+ShiftL 选择上一个,下一个) 


# 重构
Ctrl+Shift+Alt+T



# 合并为一行
Ctrl+Shift+J



# 代码模板
Ctrl+J 可查看所有


神奇的小功能Ctrl+Shift+V粘贴很早以前拷贝过的， Alt+Shift+Insert进入到列模式进行按列选中


# 查找所有Search Everywhere :
Shift+Shift



# 查找所有符号  goto by name actions >  Enter symbol name
Ctrl+Shift+Alt+N



# 宏定义 macros
`关于IDEA不能实时编译的一个临时解决办法。。。。 - MyHome - 开源中国社区`
http://my.oschina.net/fdblog/blog/172229



# IntelliJ IDEA中与Eclipse "Quick Outline"等同功能
使用Eclipse进行开发时，我喜欢用Ctrl + O快捷键打开快速概要对话框查找或浏览当前类变量和方法。
使用IntelliJ IDEA进行开发时，可以使用Navigate | File Structure菜单或Ctrl + F12快捷键打开文件结构视图查找或浏览当前类的变量或方法。
此外如果在整个项目内查找变量或方法，可以使用Navigate | Symbol菜单或 Ctrl + Alt + Shift + N快捷键打开符号查找对话框进行查找。
Alt+7


#一键到下一个错误的位置
F2


# 打开资源管理器
Ctrl+Alt+F12


# 声明 & 引用
声明  declaration Ctrl+B

引用 ??

实现 implementation Ctrl+Alt+B ->继承


# Go To 选择到目标




 jump to navigation bar
 跳转到导航栏

 declaration
 声明

 implementation
 实现

 type declaration
 类型声明

 super method
 超级方法 Ctrl+U

 test
 测试          Ctrl+Shift+T



# 导包
Alt+回车 导入包,自动修正
Ctrl+Alt+O 优化导入的类和包


# Ctrl+S自动compile: 修改快捷键并行支持Ctrl+S




方式二: 录制宏,再为宏(Edit -> Macros)设置快捷键为Ctrl+S


# 代码折叠
当前花括号折叠     Ctrl +,Ctrl -
当前花括号(含子集)折叠     Ctrl +Alt `+`,Ctrl  Ctrl +Alt ` -`
全文花括号折叠     Ctrl +Alt `+`,Ctrl  Ctrl +Alt ` -` 




技巧:

场景一: 收起 当前花括号(含子集)[ Ctrl  Ctrl +Alt ` -` ] , 再展开当前花括号 [ Ctrl + ]




场景二: 
展开当前所有  Ctrl +Alt `+`
展开文件所有  Ctrl +Shift `+`


# 新建当前window窗口
Shift+F4


# 快速打开工具窗口


Alt+F12 Terminal控制台


# `剪切板`偶尔不能与系统`剪切板`同步
可尝试使用快捷键: `Ctrl+Shift+V` 在显示的剪切板历史中,清理掉所有的纪录`delete`.
再重新进行系统复制,尝试webstorm内粘贴是否可用.


# 快速打开文件所在目录
`Alt+F1` - 5





# `File Structure`文件结构(当前文件中所有方法)
`Alt+F1` - 4
或 `Alt+7`

 
 
# 到指定行的代码
`Ctrl+G`


---
# 网摘
`★ 十大Intellij IDEA快捷键 - 西代零零发 - 博客频道 - CSDN.NET`
http://blog.csdn.net/dc_726/article/details/42784275
---
 最终榜单
 
这榜单阵容太豪华了，后几名都是如此有用，毫不示弱。
Ø  Top #10切来切去：Ctrl+Tab
Ø  Top #9选你所想：Ctrl+W
Ø  Top #8代码生成：Template/Postfix +Tab
Ø  Top #7发号施令：Ctrl+Shift+A
Ø  Top #6无处藏身：Shift+Shift
Ø  Top #5自动完成：Ctrl+Shift+Enter
Ø  Top #4创造万物：Alt+Insert
太难割舍，前三名并列吧！
Ø  Top #1智能补全：Ctrl+Shift+Space
Ø  Top #1自我修复：Alt+Enter
Ø  Top #1重构一切：Ctrl+Shift+Alt+T
---


`为了更高效的开发代码，这里列出了一些webstorm的快捷键和zencoding - longteng2013的个人页面 - 开源中国社区`
http://my.oschina.net/longteng2013/blog/138010


`WebStorm: 更改默认的文件模板 | Lifespace`
http://www.lifelaf.com/blog/?p=1345



<!-- more -->

