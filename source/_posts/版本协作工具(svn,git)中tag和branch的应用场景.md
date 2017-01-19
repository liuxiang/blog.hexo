title: 版本协作工具(svn,git)中tag和branch的应用场景
date: 2015-12-23 00:00:00 #发表日期，一般不改动
categories: svn  #文章文类
tags: [svn ,git ]



---
# tag: 标记
常用于build打包后.目的是事后能随时获取到版本包的代码


# branch:分支 
## 场景一:
常用于线上版本同步分支.目的是遇到问题时,有同步与运营的代码,进行针对性问题的修改而不会受到主干开发版本的干扰.


>一定程度上也可以使用tag标记,区别在于tag标记是在主干,迁出后不方便多人协作.因为修改的代码不允许提交. 
(提交前需更新到当前分支最新状态,更新后又包含了日常开发代码,不能用于版本补丁)
 适用单人修改问题,补丁上线后,更新代码,再提交修改问题代码到当前分支.


## 场景二: 
新技术或实验性功能,可迁出branch分支进行,如果结果满意,分支合并到主干.不满意,废弃即可,不影响主干能力.
>需要注意的是,修改的范围,如果可预见造成大规模的冲突或目录重构等,就不建议分支主干并行开发,因为带来的合并冲突解决会非常的困难.


---
## git打tag
使用 `git tag` 命令来添加新标签
`git tag -a v1.4 -m 'version 1.4'`
使用 `git push` 命令来将标签推送到远程仓库
`git push origin v1.4:v1.4`


## git建分支
`创建分支`    git branch test



## git分支合并
* 怎样手动合并此 Pull Request


git checkout master
git pull http://git.oschina.net/liuxiang7/auction.git auctionbak
git push origin master


## git分支控制常规命令
`查看远程分支`    git branch -a 
`查看本地分支`    git branch
`创建分支`    git branch test
`查看本地分支`    git branch
`线面是把分支推到远程分支`    git push origin test
`切换分支到test`    git checkout test
`删除本地分支`   git branch -d test
`看出来origin其实就是远程的git地址的一个别名` git remote  -v 
 
`git 查看远程分支、本地分支、创建分支、把分支推到远程repository、删除本地分支`
http://blog.csdn.net/arkblue/article/details/9568249


`3.2 Git 分支 - 分支的新建与合并`
http://git-scm.com/book/zh/ch3-2.html



---
## svn打`Branch/Tag` 一体
`创建` 目录 右键-TortoiseSVN- 分支/标记-目标路径,备注-确定
`查看` 版本库浏览器


## 分支合并
目录 右键-TortoiseSVN-合并-选择来源的代码库-合并



`svn 命令行创建和删除 分支和tags`
http://blog.csdn.net/yangzhongxuan/article/details/7519948



`TortoiseSVN中分支和合并实践`
http://www.open-open.com/lib/view/open1346982569725.html



---


`SVN在团队项目中的使用技巧：[2]Tag操作`
http://jingyan.baidu.com/article/fa4125acbf509e28ac709290.html


`SVN之tag打包`
http://jingyan.baidu.com/article/636f38bb4738e1d6b846100f.html



<!-- more -->
