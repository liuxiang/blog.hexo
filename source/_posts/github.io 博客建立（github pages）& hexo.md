title: github.io 博客建立（github pages）& hexo
date: 2015-10-01 00:00:00 #发表日期，一般不改动
categories: blog #文章文类
tags: [博客,文章] #文章标签，多于一项时用这种格式


---
基于github pages支持，建立静态博客。使用静态博客工具hexo
依赖工具 node,git


## github的独立二级域名开通方法：
```
https://pages.github.com/
http://jingyan.baidu.com/article/ed2a5d1f3732cb09f7be1745.html
```


## hexo 完整部署过程：
`hexo你的博客` 
http://www.tuicool.com/articles/AfQnQjy/


`如何搭建一个独立博客——简明Github Pages与Hexo教程`
http://www.jianshu.com/p/05289a4bc8b2



`史上最详细“截图”搭建Hexo博客并部署到Github_百度经验` 
http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html


`hexo | 逝去的记忆 `
http://www.coolpython.cn/category/hexo/


`使用Hexo搭建博客`
https://blog.lmintlcx.com/post/blog-with-hexo.html




### 常用命令
```
$ npm install hexo-cli -g #npm安装hexo-cli
$ hexo init blog #hexo初始化,命名blog
$ cd blog #进入目录
$ npm install #安装依赖模块
$ hexo server #开启预览(http://localhost:4000,Press Ctrl+C to stop)


$ hexo generate #生成静态页面
$ hexo server #开启预览(http://localhost:4000,Press Ctrl+C to stop)
$ hexo deploy #发布github(依赖_config.yml中deploy-github配置)
```
--- 
输出在：hexo\public\  里面内容可以手动拷贝出来,使用git客户端工具上传到<youname>.github.io项目里面就ok了。
其它教程多见配置ssh（适合脱离git客户端使用）


### 命令上传到github
`配置SSH keys`
http://www.jianshu.com/p/05289a4bc8b2


> 安装Git - 打开Git Bash Here窗口
> 设置用户名邮箱
git config --global user.name "liuxiang"
git config --global user.email "liuxiang.1227@qq.com.com"


SSH keys

```
$   ssh


$ ssh-keygen -t rsa -C "邮件地址@youremail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>


Enter passphrase (empty for no passphrase):<输入加密串>  (不建议输入密码,hexo提交默认是无密码)
Enter same passphrase again:<再次输入加密串>
```


效果
```
$ ssh-keygen -t rsa -C "liuxiang.1227@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
/c/Users/Administrator/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa.
Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:GDZ/8mEe4W9vL+re3ZI0S16zSgWgAB23/lvQVi+BEi4 liuxiang.1227@qq.com
The key's randomart image is:
+---[RSA 2048]----+
|     .oo....     |
|       .o.o...   |
|      + Eo+ .... |
|     . =.o o. o..|
|      . S.*. o...|
|         *.+o =o.|
|          o.o* =o|
|           .+oBo.|
|           ++++++|
+----[SHA256]-----+
```


>添加SSH Key到GitHub
在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。


>1、打开本地C:\Documents and Settings\Administrator.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。
 
>2、登陆github系统。点击右上角的 Account Settings--->SSH Public keys ---> add another public keys
 
>3、把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了


>测试

```
$ ssh -T git@github.com
Hi liuxiang! You've successfully authenticated, but GitHub does not provide shell access.
```


### 扩展能力
RSS      `npm install hexo-generator-feed`
sitemap     ` npm install hexo-generator-sitemap `


### fancybox
可能有人对这个 Reading 页面中图片的 fancybox 效果感兴趣，这个是怎么做的呢。
很简单，只需要在你的文章*.md文件的头上添加 photos 项即可，然后一行行添加你要展示的照片：
 ```
layout: photo
title: 我的阅历
date: 2085-01-16 07:33:44
tags: [hexo]
photos:
- http://bruce.u.qiniudn.com/2013/11/27/reading/photos-0.jpg
- http://bruce.u.qiniudn.com/2013/11/27/reading/photos-1.jpg
```
### 主题推荐：
```
http://www.zhihu.com/question/24422335
```
<!-- more -->
