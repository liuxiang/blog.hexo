title: 记一次 git  Pull Request
date: 2015-12-15 00:00:00 #发表日期，一般不改动
categories: git  #文章文类
tags: [git , Pull Request ]



---
# 1.接收到Pull Request任务: Pull Request #1 分支同步

![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-15/82136519.jpg)


<!--


-->





# 2.收到邮件
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-15/81861364.jpg)


<!--


-->



# 3.查看怎样操作:
```
# 怎样手动合并此 Pull Request

git checkout master
   
git pull http://git.oschina.net/liuxiang7/auction.git auctionbak
 
git push origin master
```
或通过工具(TortoiseGit,eclipse Git)
* 下载待合并的目标分支 [对应  git checkout master]
*   更新来源代码库及选定分支 [对应  git pull http://git.oschina.net/liuxiang7/auction.git auctionbak ]
*   本地提交更改 行为
*   将当前分支(即待合并目标分支)状态推送至服务器 [对应  git push origin master ]


# 4.收到邮件
![]( http://7xnbs3.com1.z0.glb.clouddn.com/15-12-15/79177319.jpg)


<!-- 


-->



<!-- more -->




