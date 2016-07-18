title: 常用git命令
date: 2016-05-28 00:00:00
categories:  git命令

tags: [ git命令 ]


---


# 客户端
`Git-2.8.3-64-bit.exe`
https://git-scm.com/download/win


# github引导

```
echo "# blog.hexo" >> README.md

git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/liuxiang/blog.hexo.git
git push -u origin master  # 推送 远程仓库
```


# oschina 引导
简易的命令行入门教程:


Git 全局设置:
```
git config --global user.name "lushanlai"
git config --global user.email "417951577@qq.com"
```


创建 git 仓库:
``` 
mkdir private
cd private
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://git.oschina.net/lushanlai/private.git
git push -u origin master
```


已有项目?
``` 
cd existing_git_repo
git remote add origin https://git.oschina.net/lushanlai/private.git
git push -u origin master
```


# 本地仓库 & 推送服务器
```
git init  # 初始化目录为git仓库

git add .   # 添加所有（暂存）

git commit -m "first commit" # 提交暂存



git remote add origin  https://git.oschina.net/liuxiang7/SQL-work.git   # 设置远程仓库

git pull -v --progress "origin"  # 更新远程仓库到本地
git push -u origin master # 推送至 远程仓库

```


```
F:\work\SQL work>git init
Initialized empty Git repository in F:/work/SQL work/.git/


F:\work\SQL work>git add .
warning: LF will be replaced by CRLF in MySql-wosai/更多/game/排名.sql.
The file will have its original line endings in your working directory.


F:\work\SQL work>git commit -m "first commit"
[master (root-commit) 63697a6] first commit
warning: LF will be replaced by CRLF in MySql-wosai/更多/game/排名.sql.
The file will have its original line endings in your working directory.
 72 files changed, 11363 insertions(+)
 create mode 100644 "Asia-Sql/7\347\272\247\345\214\272\345\237\237.sql"
...




F:\work\SQL work>git.exe pull -v --progress "origin"  # 更新
Username for 'https://git.oschina.net': liuxiang.1227@qq.com
Password for 'https://liuxiang.1227@qq.com@git.oschina.net':
fatal: Couldn't find remote ref #


F:\work\SQL work>git remote add origin https://git.oschina.net/liuxiang7/SQL-work.git


F:\work\SQL work>git push -u origin master
Username for 'https://git.oschina.net': liuxiang.1227@qq.com
Password for 'https://liuxiang.1227@qq.com@git.oschina.net':
Counting objects: 77, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (70/70), done.
Writing objects: 100% (77/77), 104.78 KiB | 0 bytes/s, done.
Total 77 (delta 2), reused 0 (delta 0)
To https://git.oschina.net/liuxiang7/SQL-work.git
   2caa42f..709f26f  master -> master
Branch master set up to track remote branch master from origin.
```


---


`git常用命令 - 推酷`
http://www.tuicool.com/articles/FZBvUrn


<!-- more -->
