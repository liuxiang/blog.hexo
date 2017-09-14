title: IDE日常系列——WebStorm自动编译SCSS(配置Watchers)
date: 2016-3-25 00:00:00
categories:  WebStorm

tags: [ WebStorm,scss ]


---



# 前置条件：

需要安装`Ruby` & gem安装sass
```
gem sources -a http://gems.ruby-china.org/

gem install sass
```


# 配置：
* 添加SCSS监听Watchers任务,提示
`Please set program to run!`


*   功能位置 `Settings-Tools-File Watchers-add SCSS`
Ctrl+Shift+A `setting`
搜索 `watcher` 选择结果 `Tools File Watchers`


*   修改输出路径(默认与原文件同级):
`Arguments`:  `--no-cache --update $FileName$:$FileParentDir$\www\css\ $FileNameWithoutExtension$.css`



```
Program: C:\Ruby22-x64\bin\scss.bat
Arguments:
默认：--no-cache --update $FileName$:$FileNameWithoutExtension$.css
修改：--no-cache --update $FileName$:$FileParentDir$\www\css\$FileNameWithoutExtension$.css
```
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/2926259.jpg)

 


# 编译错误: 编码问题
```
body:before {
  white-space: pre;
  font-family: monospace;
  content: "Error: Invalid GBK character \"\xE5\"\A         on line 27 of style.scss\A \A 22: @import \"www/lib/ionic/scss/ionic\";\A 23: @import \"feedback\";\A 24: \A 25: @charset \"utf-8\";\A 26: \A 27: /*增加系统样式*/\A 28: @mixin add-col-percent($percent:30%){\A 29:   -webkit-box-flex: 0;\A 30:   -webkit-flex: 0 0 $percent;\A 31:   -moz-box-flex: 0;\A 32:   -moz-flex: 0 0 $percent;"; }
```


# 解决办法
在scss文件的`头部`加一行 ` @charset "utf-8" `
注意:必须第一行,第一行空白第二行 ` @charset "utf-8" `都不行


**参考**
`webstorm scss 自动编译添加css3前缀`

http://jingyan.baidu.com/article/3c343ff7e0a39d0d3779633c.html


`WebStorm开启Scss的Source Maps功能`

http://my.oschina.net/jaycreater/blog/276741?fromerr=OtkJ5fJd


`如何为WebStorm设置SASS的File Watchers？-前端集合`
http://geek100.com/2608/


`ruby编译scss出现invalid GBK错误`
https://segmentfault.com/a/1190000002932352


<!-- more -->
