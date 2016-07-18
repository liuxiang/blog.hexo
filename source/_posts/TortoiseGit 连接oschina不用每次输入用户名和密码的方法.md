title: TortoiseGit 连接oschina不用每次输入用户名和密码的方法
date: 2015-12-25 00:00:02 #发表日期，一般不改动
categories: git   #文章文类
tags: [git ]



---
# 修改配置 
配置 `C:\Documents and Settings\Administrator\.gitconfig`

```

[credential]     
    helper = store 
```


# 记住用户名和密码
下次再输入用户名和密码时，git就会记住，从而在C:\Documents and Settings\Administrator\ 目录下形成一个  .git-credentials

```
https://username:12345678@git.oschina.net  

```


**参考**
`TortoiseGit 连接oschina不用每次输入用户名和密码的方法`
http://blog.csdn.net/liukang325/article/details/24105913


`TortoiseGit(乌龟git)保存用户名密码的方法`
http://blog.csdn.net/kongjiea/article/details/40585869


<!-- more -->
