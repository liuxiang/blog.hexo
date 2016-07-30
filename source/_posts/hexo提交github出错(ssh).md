title: hexo提交github出错(ssh)

date: 2016-3-14 00:00:00
categories:  hexo  


tags: [ hexo   ,  github pages,  ssh ]


---


# `hexo d` 提交github出错(ssh)
```
On branch master
nothing to commit, working directory clean
Warning: Permanently added the RSA host key for IP address '192.30.252.128' to the list of known hosts.
ERROR: Permission to liuxiang/liuxiang.github.io.git denied to lushanlai.
fatal: Could not read from remote repository.


Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Warning: Permanently added the RSA host key for IP address '192.30.252.128' to the list of known hosts.
ERROR: Permission to liuxiang/liuxiang.github.io.git denied to lushanlai.
fatal: Could not read from remote repository.


Please make sure you have the correct access rights
and the repository exists.


    at ChildProcess.<anonymous> (F:\Tool\node\hexo\blog\node_modules\hexo-deployer-git\node_modules\hexo-util\lib\spawn.js:42:17)
    at emitTwo (events.js:87:13)
    at ChildProcess.emit (events.js:172:7)
    at maybeClose (internal/child_process.js:817:16)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:211:5)
```


```
nothing to commit, working directory clean
fatal: Could not read from remote repository.
```


# 解决
- 重新本地生成ssh-key
```
$ ssh-keygen -t rsa -C "邮件地址@youremail.com"

Generating public/private rsa key pair.
Enter file in which to save the key (/Users/your_user_directory/.ssh/id_rsa):<回车就好>
Enter passphrase (empty for no passphrase):<输入加密串>  (不建议输入密码,hexo提交默认是无密码)
Enter same passphrase again:<再次输入加密串>
```
- 更新,github  ` https://github.com/settings/ssh`


- 测试连接 ` ssh -T git@github.com `


- hexo重新提交 `hexo d`


---
#  上传操作  hexo d  报错spawn git ENOENT
```
Error: spawn git ENOENT
    at exports._errnoException (util.js:746:11)
    at Process.ChildProcess._handle.onexit (child_process.js:1053:32)
    at child_process.js:1144:20)
    at process._tickCallback (node.js:355:11)
```
未添加Git环境变量引起，添加Git与git管理库的环境变量即可；
```
D:\Git\bin;D:\Git\libexec\git-core
 ```
`hexo 部署至Git遇到的坑 - 简书`
http://www.jianshu.com/p/67c57c70f275

<!-- more -->