title:  jetbrains IntelliJ/Webstorm svn插件初始化


date: 2017-5-9 00:00:00
tags: [ jetbrains ]



---


#   subversion 工具 下载


http://subversion.apache.org/packages.html#windows
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/88396036.jpg)



---
- **subversion(支持控制台Command) **
https://tortoisesvn.net/downloads.html  `TortoiseSVN-1.9.5.27581-x64-svn-1.9.5.msi`

使用的是TortoiseSVN-1.9.4.27285-x64-svn-1.9.4，安装过程需要注意的是，默认安装没有选择”command line client tools”
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/25113416.jpg)



- **subversion **  绿色免安装
http://subversion.apache.org/download.cgi#recommended-release  `subversion-1.9.5.zip`
**命令行支持，配置环境变量**：path += `F:\Tool\SVN,CVS\Apache-Subversion-1.9.5\bin`


-  ** sliksvn **  ： ` Slik-Subversion-1.9.5-x64.msi `
https://sliksvn.com/download/



---
# 配置
`Ctrl + Alt + S`
** Use Command line client ** ：  `svn` 或 `C:\Program Files\SlikSvn\bin\svn.exe`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-5-31/56881942.jpg)


---
**参考**
`Windows下使用svn命令行.md`



`Webstorm2016使用技巧——SVN插件使用 - 博客频道 - CSDN.NET`

http://blog.csdn.net/hysh_keystone/article/details/52013789


`Webstorm SVN配置_屋里屋外点点_新浪博客`
http://blog.sina.com.cn/s/blog_9984f2fb0102w8av.html


`webstorm上svn的安装使用`
http://blog.csdn.net/jgj0129/article/details/52757115


`WebStorm配置svn共享、检出项目-博客-云栖社区-阿里云`
https://yq.aliyun.com/articles/46981
