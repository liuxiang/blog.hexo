title: windows svn 备份命令
date: 2016-01-14 00:00:00
tags: [svn]
 
---


# 备份

```
svnadmin dump "D:\VisualSVN Repositories\code.wosai"   > "D:\VisualSVN Repositories-dump\code.wosai.dump"
svnadmin dump "D:\VisualSVN Repositories\doc.wosai"   > "D:\VisualSVN Repositories-dump\doc.wosai.dump"
svnadmin dump "D:\VisualSVN Repositories\com.wosai.cocos"   > "D:\VisualSVN Repositories-dump\com.wosai.cocos.dump"
svnadmin dump "D:\VisualSVN Repositories\Unity3D"   > "D:\VisualSVN Repositories-dump\Unity3D.dump"
svnadmin dump "D:\VisualSVN Repositories\wosai-ios-archive"   > "D:\VisualSVN Repositories-dump\wosai-ios-archive.dump"
``` 


# 脚本 bat
```
svnadmin-dump.bat

```


# win 计划任务






# 恢复
命令
```
svnadmin load  "D:\VisualSVN Repositories\code.wosai"   > "D:\VisualSVN Repositories-dump\code.wosai.dump"

```


工具




**参考**
`windows下如何备份导出SVN库、导入SVN库_百度经验`
http://jingyan.baidu.com/article/ca00d56c900dd8e99eebcf81.html


`svn 版本库的备份和还原-EnderViking-ChinaUnix博客`
http://blog.chinaunix.net/uid-11618169-id-2865341.html


`svn 版本库的备份和还原-EnderViking-ChinaUnix博客`
http://blog.chinaunix.net/uid-11618169-id-2865341.html/


`linux svn迁移备份的三种方法 | 爱积累爱分享`
http://www.iitshare.com/linux-svn-migration.html


`SVN版本库的迁移 dump的详细使用_砖头不离身_新浪博客`
http://blog.sina.com.cn/s/blog_668aae7801017gn5.html



`svnadmin dump备份工具-kidking2010-ITPUB博客`
http://blog.itpub.net/23141985/viewspace-713631/


---



`svnadmin hotcopy` 目录备份策略
```
@echo off
 
set obj=wosai-ios-archive
set dateTime=%date:~0,4%-%date:~5,2%-%date:~8,2% %time:~0,2%%time:~3,2%%time:~6,2%
set bakdir=D:\VisualSVN Repositories-dump\%dateTime%.%obj%.hotcopy
echo 开始拷贝%obj%
svnadmin hotcopy "D:\VisualSVN Repositories\%obj%" "%bakdir%" --clean-logs
echo 开始压缩%obj%
set bakzipdir=D:\VisualSVN Repositories-dump\hostcopy_zip\%dateTime%.%obj%.hotcopy
"C:\Program Files\7-Zip\7z.exe" a -tzip "%bakzipdir%.zip" "%bakdir%"
 
set obj=code.wosai
set dateTime=%date:~0,4%-%date:~5,2%-%date:~8,2% %time:~0,2%%time:~3,2%%time:~6,2%
set bakdir=D:\VisualSVN Repositories-dump\%dateTime%.%obj%.hotcopy
echo 开始拷贝%obj%
svnadmin hotcopy "D:\VisualSVN Repositories\%obj%" "%bakdir%" --clean-logs
echo 开始压缩%obj%
set bakzipdir=D:\VisualSVN Repositories-dump\hostcopy_zip\%dateTime%.%obj%.hotcopy
"C:\Program Files\7-Zip\7z.exe" a -tzip "%bakzipdir%.zip" "%bakdir%"
 
 
set obj=com.wosai.cocos
set dateTime=%date:~0,4%-%date:~5,2%-%date:~8,2% %time:~0,2%%time:~3,2%%time:~6,2%
set bakdir=D:\VisualSVN Repositories-dump\%dateTime%.%obj%.hotcopy
echo 开始拷贝%obj%
svnadmin hotcopy "D:\VisualSVN Repositories\%obj%" "%bakdir%" --clean-logs
echo 开始压缩%obj%
set bakzipdir=D:\VisualSVN Repositories-dump\hostcopy_zip\%dateTime%.%obj%.hotcopy
"C:\Program Files\7-Zip\7z.exe" a -tzip "%bakzipdir%.zip" "%bakdir%"
 
 
set obj=doc.wosai
set dateTime=%date:~0,4%-%date:~5,2%-%date:~8,2% %time:~0,2%%time:~3,2%%time:~6,2%
set bakdir=D:\VisualSVN Repositories-dump\%dateTime%.%obj%.hotcopy
echo 开始拷贝%obj%
svnadmin hotcopy "D:\VisualSVN Repositories\%obj%" "%bakdir%" --clean-logs
echo 开始压缩%obj%
set bakzipdir=D:\VisualSVN Repositories-dump\hostcopy_zip\%dateTime%.%obj%.hotcopy
"C:\Program Files\7-Zip\7z.exe" a -tzip "%bakzipdir%.zip" "%bakdir%"
 
 
set obj=Unity3D
set dateTime=%date:~0,4%-%date:~5,2%-%date:~8,2% %time:~0,2%%time:~3,2%%time:~6,2%
set bakdir=D:\VisualSVN Repositories-dump\%dateTime%.%obj%.hotcopy
echo 开始拷贝%obj%
svnadmin hotcopy "D:\VisualSVN Repositories\%obj%" "%bakdir%" --clean-logs
echo 开始压缩%obj%
set bakzipdir=D:\VisualSVN Repositories-dump\hostcopy_zip\%dateTime%.%obj%.hotcopy
"C:\Program Files\7-Zip\7z.exe" a -tzip "%bakzipdir%.zip" "%bakdir%"
 
pause
```


`bat文件压缩批量处理_写代码的喵_新浪博客`
http://blog.sina.com.cn/s/blog_6954c2c401019203.html


`Bat脚本调用7-zip对压缩文件进行解压_百度经验`
http://jingyan.baidu.com/article/e6c8503c7d35b3e54f1a1838.html



`常见问题（FAQ）`
http://www.7-zip.org/faq.html  



<!-- more -->

