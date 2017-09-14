title: svn不能上传.so包的问题
date: 2015-12-26 00:00:01 #发表日期，一般不改动
categories: svn   #文章文类
tags: [svn ]



---


# 解决办法
位置: 右键-TortoiseSVN-常规设置-Subversion-全局忽略样式


默认配置
```
*.o *.lo *.la *.al .libs  *.so *.so.[0-9]*  *.a *.pyc *.pyo __pycache__ *.rej *~ #*# .#* .*.swp .DS_Store [Tt]humbs.db

```
修改为

```
*.o *.lo *.la *.al .libs *.a *.pyc *.pyo __pycache__ *.rej *~ #*# .#* .*.swp .DS_Store [Tt]humbs.db

```


![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-2-23/36851531.jpg)


**参考**
`svn不能上传*.so包的问题`
http://blog.csdn.net/wangchun8926/article/details/43196721


`SVN 如何提交 SO 库文件`
http://blog.csdn.net/hexiaoxiao_love/article/details/10251053


`Xcode中SVN不能提交.a文件的解决方法`
http://blog.csdn.net/itianyi/article/details/39401017




<!-- more -->
