
title: linux清空文件/清理内容
date: 2017-4-30 00:00:00
tags: [ linux ]


---


`>` 覆盖文件
`>>` 追加文件


#    test.txt 测试
```
[root@iZ23j23f6aaZ ~]# echo 'hello' > test.txt
[root@iZ23j23f6aaZ ~]# ll
total 1484
-rw-r--r-- 1 root root 1506593 Dec 17 14:53 atlassian-jira-6.3.6.tar.gz
drwxr-xr-x 2 root root    4096 Jul 13  2015 cfg
drwxr-xr-x 2 root root    4096 Dec 10 15:34 –p
-rw-r--r-- 1 root root       6 Apr 30 09:28 test.txt


[root@iZ23j23f6aaZ ~]# echo null > test.txt
[root@iZ23j23f6aaZ ~]# ll
total 1484
-rw-r--r-- 1 root root 1506593 Dec 17 14:53 atlassian-jira-6.3.6.tar.gz
drwxr-xr-x 2 root root    4096 Jul 13  2015 cfg
drwxr-xr-x 2 root root    4096 Dec 10 15:34 –p
-rw-r--r-- 1 root root       5 Apr 30 09:28 test.txt


[root@iZ23j23f6aaZ ~]# echo '' > test.txt

[root@iZ23j23f6aaZ ~]# ll
total 1484
-rw-r--r-- 1 root root 1506593 Dec 17 14:53 atlassian-jira-6.3.6.tar.gz
drwxr-xr-x 2 root root    4096 Jul 13  2015 cfg
drwxr-xr-x 2 root root    4096 Dec 10 15:34 –p
-rw-r--r-- 1 root root       1 Apr 30 09:28 test.txt


[root@iZ23j23f6aaZ ~]# cat null > test.txt
cat: null: No such file or directory
[root@iZ23j23f6aaZ ~]# ll
total 1480
-rw-r--r-- 1 root root 1506593 Dec 17 14:53 atlassian-jira-6.3.6.tar.gz
drwxr-xr-x 2 root root    4096 Jul 13  2015 cfg
drwxr-xr-x 2 root root    4096 Dec 10 15:34 –p
-rw-r--r-- 1 root root       0 Apr 30 09:29 test.txt


```
- ` echo '' > test.txt`    结果1kb
- ` cat null > test.txt` 结果0 kb


#    access.log  测试
```
[root@iZ23j23f6aaZ logs]# ll
total 1304
-rw-r--r-- 1 root   root 613639 Apr 30 09:33 access_liux.log
-rw-r--r-- 1 nobody root  58073 Apr 30 09:36 access.log
-rw-r--r-- 1 nobody root 639126 Apr 30 07:53 error.log
-rw-r--r-- 1 root   root      6 Apr  8 21:12 nginx.pid


[root@iZ23j23f6aaZ logs]# echo '' > access.log 
[root@iZ23j23f6aaZ logs]# ll
total 1244
-rw-r--r-- 1 root   root 613639 Apr 30 09:33 access_liux.log
-rw-r--r-- 1 nobody root   1102 Apr 30 09:36 access.log
-rw-r--r-- 1 nobody root 639126 Apr 30 07:53 error.log
-rw-r--r-- 1 root   root      6 Apr  8 21:12 nginx.pid
[root@iZ23j23f6aaZ logs]# ll
total 1256
-rw-r--r-- 1 root   root 613639 Apr 30 09:33 access_liux.log
-rw-r--r-- 1 nobody root  14806 Apr 30 09:36 access.log
-rw-r--r-- 1 nobody root 639126 Apr 30 07:53 error.log
-rw-r--r-- 1 root   root      6 Apr  8 21:12 nginx.pid


[root@iZ23j23f6aaZ logs]# cat null > access.log
cat: null: No such file or directory
[root@iZ23j23f6aaZ logs]# ll
total 1244
-rw-r--r-- 1 root   root 613639 Apr 30 09:33 access_liux.log
-rw-r--r-- 1 nobody root   2190 Apr 30 09:36 access.log
-rw-r--r-- 1 nobody root 639126 Apr 30 07:53 error.log
-rw-r--r-- 1 root   root      6 Apr  8 21:12 nginx.pid
[root@iZ23j23f6aaZ logs]# ll
total 1252
-rw-r--r-- 1 root   root 613639 Apr 30 09:33 access_liux.log
-rw-r--r-- 1 nobody root  11339 Apr 30 09:36 access.log
-rw-r--r-- 1 nobody root 639126 Apr 30 07:53 error.log
-rw-r--r-- 1 root   root      6 Apr  8 21:12 nginx.pid


[root@iZ23j23f6aaZ logs]# cat null > access.log
cat: null: No such file or directory
[root@iZ23j23f6aaZ logs]# ll
total 1244
-rw-r--r-- 1 root   root 613639 Apr 30 09:33 access_liux.log
-rw-r--r-- 1 nobody root   1801 Apr 30 09:36 access.log
-rw-r--r-- 1 nobody root 639126 Apr 30 07:53 error.log
-rw-r--r-- 1 root   root      6 Apr  8 21:12 nginx.pid
[root@iZ23j23f6aaZ logs]# 
```




```
[root@iZ23ult4m4yZ ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G  5.2G   14G  28% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
/dev/xvdb1       20G  4.6G   15G  25% /mnt


[root@iZ23ult4m4yZ ~]# cd /usr/local/nginx/logs/
[root@iZ23ult4m4yZ logs]# ll
total 1025708
-rw-r--r-- 1 root   root 702671189 Apr 28 11:21 access_2017-04-27_11:21:53.log
-rw-r--r-- 1 nobody root  77063276 Apr 29 00:00 access_2017-04-28_00:00:01.log
-rw-r--r-- 1 nobody root 143218093 Apr 30 00:00 access_2017-04-29_00:00:01.log
-rw-r--r-- 1 nobody root  49736274 Apr 30 09:39 access.log
-rw-r--r-- 1 nobody root  76566363 Apr 30 09:39 error.log
-rw-r--r-- 1 root   root         6 Apr 23 20:18 nginx.pid
[root@iZ23ult4m4yZ logs]# du -sh *
671M access_2017-04-27_11:21:53.log
74M access_2017-04-28_00:00:01.log
137M access_2017-04-29_00:00:01.log
48M access.log
74M error.log
4.0K nginx.pid
[root@iZ23ult4m4yZ logs]# cat null > access_2017-04-27_11\:21\:53.log 
cat: null: No such file or directory
[root@iZ23ult4m4yZ logs]# du -sh *
0 access_2017-04-27_11:21:53.log
74M access_2017-04-28_00:00:01.log
137M access_2017-04-29_00:00:01.log
48M access.log
74M error.log
4.0K nginx.pid
[root@iZ23ult4m4yZ logs]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G  5.2G   14G  28% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
/dev/xvdb1       20G  4.0G   15G  21% /mnt






[root@iZ23ult4m4yZ logs]# ll
total 123716
-rw-r--r-- 1 root   root        0 Apr 30 09:40 access_2017-04-27_11:21:53.log
-rw-r--r-- 1 nobody root        0 Apr 30 09:41 access_2017-04-28_00:00:01.log
-rw-r--r-- 1 nobody root        0 Apr 30 09:41 access_2017-04-29_00:00:01.log
-rw-r--r-- 1 nobody root 49978683 Apr 30 09:41 access.log
-rw-r--r-- 1 nobody root 76566363 Apr 30 09:39 error.log
-rw-r--r-- 1 root   root        6 Apr 23 20:18 nginx.pid
[root@iZ23ult4m4yZ logs]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G  5.2G   14G  28% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
/dev/xvdb1       20G  3.8G   15G  20% /mnt


[root@iZ23ult4m4yZ logs]# cat null > catalina.out 
cat: null: No such file or directory
[root@iZ23ult4m4yZ logs]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       20G  5.2G   14G  28% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
/dev/xvdb1       20G  324M   19G   2% /mnt
```