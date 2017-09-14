


title:  Java Stream调试器
date: 2017-07-21 00:00:00
tags: [idea, Stream ]




---


# Java Stream调试器

在某些方面，Stream API比传统的循环方法更好：它充分利用现代多核架构，并且可以以声明的方式处理数据。还有一点，这种方法有助于避免国家的问题，并且写入的代码看起来更加优雅。但是，它有一定的缺点：代码有时确实难以阅读，理解，当然也可以调试。
这个插件在这里修改，并提供可能遇到的问题的解决方案。它通过添加跟踪当前流链接按钮来扩展调试器工具窗口，当调试器停在Stream API调用链中时，该按钮将变为活动状态。
![](https://blog.jetbrains.com/idea/files/2017/05/Screen-Shot-2017-05-11-at-15.06.58.png)





单击它之后，将评估当前的数据流，并且可以看到从第一次调用到最后一个调用的每个元素究竟发生了什么样的变化，随着所有步骤的传递逐渐发生变化： ![]( https://blog.jetbrains.com/idea/files/2017/05/Screen-Shot-2017-05-11-at-15.06.18.png )






左下角的“ 拆分模式”按钮可让您选择是要一次还是单独查看所有操作：
![]( https://blog.jetbrains.com/idea/files/2017/05/Screen-Shot-2017-05-11-at-15.04.39.png )






在后一种模式下，您可以使用顶部的选项卡手动切换操作。
该插件仍在开发中，所以期待在这里和那里有几个故障，当然，我们非常感谢您的反馈，包括错误报告，我们已经设置了一个问题跟踪器。




# 一般使用说明
观看以下短动画以查看操作中的功能：

![]( https://raw.githubusercontent.com/Roenke/static/master/screen_shot_2017-05-11_at_15.07.27.gif)


---




**参考**
` Java Stream Debugger :: JetBrains Plugin Repository `
https://plugins.jetbrains.com/plugin/9696-java-stream-debugger
