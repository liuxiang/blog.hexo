title: 手把手搭blog & github pages,hexo

date: 2016-3-13 00:00:00
categories:   blog  


tags: [ blog   ,  github pages,  hexo ]


---


> 基于github pages支持 & hexo  静态博客 生成器


# 收益
- 学习留存率
http://liuxiang.github.io/2015/12/18/%E5%AD%A6%E4%B9%A0%E5%86%85%E5%AE%B9%E5%B9%B3%E5%9D%87%E7%95%99%E5%AD%98%E7%8E%87/


- 从事行业留下点东西
- github好装逼,找工作待遇好
- 分类:留自己看,给别人看
- 被动总结,结构化知识
- 方便分享


# 一.pages.github
1.注册github账号
2.新建 [username].github.io  项目


**参考**
https://pages.github.com/


# 二.hexo 博客生成器
windows：安装 nodejs,hexo


```
$ npm install hexo-cli -g #npm安装hexo-cli
$ hexo init blog #hexo初始化,命名blog
$ cd blog #进入目录
$ npm install #安装依赖模块
$ hexo server #开启预览(http://localhost:4000,Press Ctrl+C to stop)


$ hexo generate #生成静态页面
$ hexo server #开启预览(http://localhost:4000,Press Ctrl+C to stop)
```


# 三.配置hexo提交github路径
`blog/_config.yml`
```
deploy:
  type: git
  #repository: https://github.com/liuxiang/liuxiang.github.io.git
  repository: git@github.com:liuxiang/liuxiang.github.io.git
  branch: master
```


# 四.配置github代码提交 ssh信任（作用：免账号提交代码，集成hexo）
`配置SSH keys`
http://www.jianshu.com/p/05289a4bc8b2


> 安装Git - 打开Git Bash Here窗口
> 设置用户名邮箱
git config --global user.name "liuxiang"
git config --global user.email "liuxiang.1227@qq.com.com"


SSH keys

- 介绍
```
$ ssh-keygen -t rsa -C "邮件地址@youremail.com"

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>


Enter passphrase (empty for no passphrase):<输入加密串>  (不建议输入密码,hexo提交默认是无密码)
Enter same passphrase again:<再次输入加密串>
```


- 操作
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


-添加SSH Key到GitHub
在本机设置SSH Key之后，需要添加到GitHub上，以完成SSH链接的设置。


>1、打开本地C:\Documents and Settings\Administrator.ssh\id_rsa.pub文件。此文件里面内容为刚才生成人密钥。如果看不到这个文件，你需要设置显示隐藏文件。准确的复制这个文件的内容，才能保证设置的成功。
 
>2、登陆github系统。点击右上角的 Account Settings--->SSH Public keys ---> add another public keys
 
>3、把你本地生成的密钥复制到里面（key文本框中）， 点击 add key 就ok了


>测试

```
$ ssh -T git@github.com
Hi liuxiang! You've successfully authenticated, but GitHub does not provide shell access.
```

 
## 五.hexo提交发布内容(public)
` hexo deploy` #发布github(依赖_config.yml中deploy-github配置)


** 错误**
`hexo deploy命令显示ERROR Deployer not found : github`
**解决办法**
`npm install hexo-deployer-git --save`


**重试**
` hexo deploy` #发布github(依赖_config.yml中deploy-github配置)



## 测试访问 `****.github.io`



---
# 六.更新`hexo`主题模板
github搜：hexo-theme
https://github.com/search?o=desc&q=hexo-theme&s=stars&type=Repositories&utf8=%E2%9C%93
如：`hexo-theme-yilia`


- `下载&位置`: `blog/themes`
- `更新主题模板应用` 修改hexo根目录下的 _config.yml ： theme: yilia
- `重新编译&本地服务` `hexo g & hexo s`


**出错**
```
ERROR Process failed: layout/.DS_Store
TypeError: Cannot read property 'compile' of undefined
```
**解决办法**
```
cd 进到你使用的theme对应的目录，再进到layout/和layout/_partial/下.
分别执行rm .DS_Store
```
**参考**  https://segmentfault.com/q/1010000004533667


- 重试>重新编译&本地服务 `hexo g & hexo s`


# 提交blog更新

` hexo deploy` #发布github(依赖_config.yml中deploy-github配置)



---
# 保留hexo工程代码，提交至github
- github 新建工程`blog.hexo`
- 将本地blog工程提交
```
echo "# blog.hexo" >> README.md

git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/liuxiang/blog.hexo.git
git push -u origin master
```
---


# hexo使用


- 指定文章`标题`&`日期`&`分类`&`标签`
```
title: 文章名
date: 2016-03-13 00:00:01
categories:分类
tags: [标签1, 标签2]
```


- 增加更多
`<!-- more -->`  之前内容会显示在首页预览，之后内容只能在点击文章标题进入后才可看到


- 插件
`统计访问:不蒜子` http://service.ibruce.info/

`更多` https://hexo.io/plugins/
`搜索插件` Hexo 中有用户基础的 Swiftype 和 Hexo 官网使用的 Algolia


## markdown格式，编写指导
`马克飞象` https://maxiang.io/
`作业部落` https://www.zybuluo.com/mdeditor
`七牛图床`  http://yotuku.cn/


---


**参考**
`hexo你的博客` 
http://www.tuicool.com/articles/AfQnQjy/


`如何搭建一个独立博客——简明Github Pages与Hexo教程`
http://www.jianshu.com/p/05289a4bc8b2



`Hexo搭建Github静态博客 - 金石开 - 博客园`
http://www.cnblogs.com/zhcncn/p/4097881.html


`史上最详细“截图”搭建Hexo博客并部署到Github_百度经验` 
http://jingyan.baidu.com/article/d8072ac47aca0fec95cefd2d.html


`hexo | 逝去的记忆 `
http://www.coolpython.cn/category/hexo/


`使用Hexo搭建博客`
https://blog.lmintlcx.com/post/blog-with-hexo.html


`hexo主题推荐 `

http://www.zhihu.com/question/24422335


`StaticGen开源静态网站 - StaticGen`
http://www.staticgen.com/


`手把手教你用github pages搭建博客`
http://www.woaitqs.cc/blog/2016/06/08/blog-seo-baidu.html


---
`hexo YAML [分类 & 标签]编译问题`

http://liuxiang.github.io/2015/11/30/hexo%20YAML%20[%E5%88%86%E7%B1%BB%20&%20%E6%A0%87%E7%AD%BE]%E7%BC%96%E8%AF%91%E9%97%AE%E9%A2%98/


`markdown 图片注释`
http://liuxiang.github.io/2016/02/23/markdown%20%E5%9B%BE%E7%89%87%E6%B3%A8%E9%87%8A/



<!-- more -->


