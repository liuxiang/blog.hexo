
title:  linux alias


date: 2017-05-21 00:00:00
tags: [ alias ]



---
# linux 关闭进程的集中方式-alias

- 进程，kill
`kill -9 <pid>`
```
[root@iZ23j23f6aaZ ~]# ps -ef|grep nginx|grep -v grep
nobody   25268 28893  0 May11 ?        05:52:46 nginx: worker process
root     28893     1  0 Apr08 ?        00:00:00 nginx: master process ./nginx


[root@iZ23j23f6aaZ ~]# pidof nginx
28893 25268


[root@iZ23j23f6aaZ ~]# pgrep nginx
25268
28893
```


- 依据服务名， Xargs接受管道符参数 ，kill
`ps -ef | grep <p_name> | grep -v grep |  xargs kill`
```
ps -ef | grep nginx | grep -v grep | awk '{print $2}' |  xargs kill  -9
```


- 系统别名，kill
`alias killng=" ps -ef | grep nginx | grep -v grep | awk '{print $2}' |  xargs kill  -9 "`


---
# 显示shells集合 【 Shell有这么几种，sh、bash、csh】
```
[root@iZ23j23f6aaZ ~]# cat /etc/shells
/bin/sh
/bin/bash
/sbin/nologin
/bin/dash
/bin/tcsh
/bin/csh
```
- Mac 多了一个 `zsh`

 Linux 系统和 OS X 系统的默认 Shell 都是 bash，但是真正强大的 Shell 是深藏不露的 zsh， 这货绝对是马车中的跑车，跑车中的飞行车，史称『终极 Shell』

快速上手的zsh项目，叫做「oh my zsh」
Github 网址是： https://github.com/robbyrussell/oh-my-zsh


# 查看已经配置的`alias`
```
[root@iZ23j23f6aaZ ~]# alias
alias catalog='tail -f ${CATALINA_HOME}/logs/catalina.out'
alias cfg='cd ${CFG_HOME}'
alias class='cd ${CATALINA_HOME}/webapps/server/WEB-INF/classes'
alias cp='cp -i'
alias help='sh ${CFG_HOME}/server.sh help'
alias k12='cd /usr/local/apache-tomcat-7.0.59/bin;./shutdown.sh;sleep 1;./startup.sh'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias local='cd ${CATALINA_HOME}/local'
alias log='tail -f ${CATALINA_HOME}/local/server.log'
alias logs='cd ${CATALINA_HOME}/logs'
alias ls='ls --color=auto'
alias mv='mv -i'
alias nginx='/usr/local/nginx/sbin/nginx'
alias nginxid='cat /usr/local/nginx/logs/nginx.pid'
alias pc='cd /usr/local/apache-tomcat-7.0.62/bin;./shutdown.sh;sleep 1;./startup.sh'
alias restart='sh ${CFG_HOME}/server.sh restart'
alias rm='rm -i'
alias start='sh ${CFG_HOME}/server.sh start'
alias status='sh ${CFG_HOME}/server.sh status'
alias stop='sh ${CFG_HOME}/server.sh stop'
alias version='java Version'
alias webapps='cd ${CATALINA_HOME}/webapps'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'
```


---
# 安装`zsh`
```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```
```
➜  ~ ll
total 0
drwx------@  3 liuxiang  staff   102B  7 24 13:26 Applications
drwx------+ 11 liuxiang  staff   374B  7 25 11:19 Desktop
drwx------+  4 liuxiang  staff   136B  7 24 14:31 Documents
drwx------+ 16 liuxiang  staff   544B  7 26 11:25 Downloads
drwx------@ 58 liuxiang  staff   1.9K  7 24 21:38 Library
drwx------+  3 liuxiang  staff   102B  7 24 11:02 Movies
drwx------+  4 liuxiang  staff   136B  7 25 09:08 Music
drwx------+  3 liuxiang  staff   102B  7 24 11:02 Pictures
drwxr-xr-x+  5 liuxiang  staff   170B  7 24 11:02 Public
```
`终极 Shell——ZSH - MacTalk - 知乎专栏 `
https://zhuanlan.zhihu.com/p/19556676?columnSlug=mactalk


`robbyrussell/oh-my-zsh `
https://github.com/robbyrussell/oh-my-zsh


`终极 Shell——ZSH - MacTalk - 知乎专栏 `  -`autojump`
https://zhuanlan.zhihu.com/p/19556676?columnSlug=mactalk


---
**参考**


`【转】linux下杀死进程（kill）的N种方法`

http://blog.csdn.net/andy572633/article/details/7211546



`Oh My Zsh`
http://ohmyz.sh/


`终极 Shell——ZSH`

https://zhuanlan.zhihu.com/p/19556676?columnSlug=mactalk



