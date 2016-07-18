title: mac权限管理
date: 2016-3-2 00:00:02
categories:   mac

tags: [ mac,权限管理 ]


---

# 操作无权限提示

`permission denied`


# 查看权限
`ls -l`


# 修改权限
`sudo chown $YOUR_USER -R /use/lib/node_modules`
`sudo chown -R  $USER  /use/local`


# 常见权限
`sudo chown -R $USER /user/<userName>`

`sudo chown -R $USER /use/local`


# 其它
```
1：改变拥有者和群组
　　命令：chown mail:mail server.log
2：改变文件拥有者和群组
　　命令：chown root: server.log
3：改变文件群组
　　命令：chown :mail server.log
4：改变指定目录以及其子目录下的所有文件的拥有者和群组
　　命令：chown -R -v root:mail test6
-R 处理指定目录以及其子目录下的所有文件
　　-v 显示详细的处理信息
```
`linux如何使用chown改变权限？`
http://zhidao.baidu.com/link?url=tgbohDzcEJI7TAcrsCDgich6inxF8xVRpINL1IxNceVlAd7KB-zbt5l0xCZwf5hBK4Pc_fJbdGLJilZI2MCo3q


**参考**
`Aral Balkan — npm install -g — ERR! Please try running this command again as root/Administrator.`
https://ar.al/scribbles/npm-install-g-please-try-running-this-command-again-as-root-administrator/


`Error: EACCES, permission denied in command #yo angular`
http://stackoverflow.com/questions/25945447/error-eacces-permission-denied-in-command-yo-angular


`Yo Webapp: Error EACCES, permission denied 'Gruntfile.js'`
http://stackoverflow.com/questions/24355027/yo-webapp-error-eacces-permission-denied-gruntfile-js


`每天一个linux命令（30）: chown命令`
http://www.cnblogs.com/peida/archive/2012/12/04/2800684.html


<!-- more -->
