title: bower问题—bower install 无法安装组件
date: 2016-2-23 00:00:02 #发表日期，一般不改动
categories:  bower  #文章文类

tags: [bower ]


---
# bower install 无法安装组件
```
 C:\Users\Administrator\Desktop\***>bower install
bower ng-file-upload#~11.2.3    ENOGIT git is not installed or not in the PATH 
```


原因是,组件存在git上,下载依赖git客户端



# 处理办法:
* 下载` git for windows` https://git-for-windows.github.io
* 安装&配置Path
```
set PATH=%PATH%;C:\Program Files\Git\bin

```


**参考**
`Bower : ENOGIT git is not installed or not in the PATH - B.H - SegmentFault`

https://segmentfault.com/a/1190000000535410


`Bower: ENOGIT Git is not installed or not in the PATH - Stack Overflow`
http://stackoverflow.com/questions/20666989/bower-enogit-git-is-not-installed-or-not-in-the-path


<!-- more -->
