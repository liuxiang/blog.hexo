title:  linux vi 行号/位置/查找/区间/输出/过滤/下载


date: 2017-5-18 00:00:00
tags: [ linux ]



---


# 行号

显示行号 `:nu`
全文显示 `:set nu`
- 全文行数
```
[root@iZ23huf4wo4Z logs]# wc -l localhost_access_log.2017-05-18.txt 
770460 localhost_access_log.2017-05-18.txt
```


# 位置

`gg`  跳到首一行

`GG`  跳到尾一行



# 查找（相对光标所在位置）
`/content`  （自上往下）
- 下一个`n`
- 上一个`Shift+n`


`?content`  （自 下 往 上 ）
-  上 一个`n`
-  下 一个`Shift+n`


---
# 打印出起始行和结束行之间的内容
`sed -n  '210048,210049p'  catalina.out`


- 输出文件
`sed -n  '210048,312712p'  catalina.out > catalina.out.0209.12`


# 过滤打印
`cat catalina.out.0209.12 | grep 操作计时`
```
[k12Service]  操作计时：188ms  类名: com.wosai.teach.restful.knowPoint.KnowPointLoadExpReadInterFace  方法名:getMain() result:
[k12Service]  操作计时：29ms  类名: com.wosai.teach.restful.sys.SysConfigInterface  方法名:sysConfig() result:
[k12Service]  操作计时：120ms  类名: com.wosai.teach.restful.order.FindProductListInterface  方法名:getMain() result:
[k12Service]  操作计时：112ms  类名: com.wosai.teach.restful.order.FindProductListInterface  方法名:getMain() result:
[k12Service]  操作计时：224ms  类名: com.wosai.teach.restful.knowPoint.KnowPointLoadExpReadInterFace  方法名:getMain() result:
```
- 输出文件
`cat catalina.out.0209.12 | grep 操作计时 > catalina.out.0209.12.grep`
```
[root@iZ23ult4m4yZ logs]# vi catalina.out
catalina.out               catalina.out.0209.12       catalina.out.0209.12.grep 
```
- 下载文件
`sz catalina.out.0209.12.grep`


---
` linux vi 查找 - 疯狂程序员 - 博客频道 - CSDN.NET `
http://blog.csdn.net/shine0181/article/details/6632597



`VIM中gg=G是什么 - 编程`
http://www.myexception.cn/program/880876.html


`在linux中使用vi 打开文件时，能显示行号吗？_百度知道`
https://zhidao.baidu.com/question/812870968035725732.html


`linux下的vim/vi永久显示行号`
http://www.07net01.com/linux/2016/04/1426154.html