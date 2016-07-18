title: 依据nginx日志，计算平台历史TPS承载
date: 2016-06-27 00:00:00

tags: [nginx日志 , TPS ]


---


# 查看符合规则的具体日志
file：`/usr/local/nginx/logs/access.log`
` #  cat access.log | grep "k12platform/service"`
```
100.97.170.63 - - [20/Jun/2016:09:32:25 +0800] "GET /k12platform/service/questionBank/examCollection///BJUI/js/jquery-1.7.2.min.js HTTP/1.0" 400 968 "http://120.55.144.43/k12platform/service/questionBank/examCollection///BJUI/js/jquery-1.7.2.min.js" "Mozilla/5.0 (Linux; U; Android 4.4.2; zh-cn; GT-I9500 Build/KOT49H) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 MQQBrowser/5.0 QQ-URL-Manager Mobile Safari/537.36"
100.97.170.78 - - [20/Jun/2016:09:32:25 +0800] "GET /k12platform/service/questionBank/examCollection///BJUI/js/bjui-slidebar.js HTTP/1.0" 400 968 "http://120.55.144.43/k12platform/service/questionBank/examCollection///BJUI/js/bjui-slidebar.js" "Mozilla/5.0 (Linux; U; Android 4.4.2; zh-cn; GT-I9500 Build/KOT49H) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 MQQBrowser/5.0 QQ-URL-Manager Mobile Safari/537.36"
100.97.169.180 - - [20/Jun/2016:09:32:25 +0800] "GET /k12platform/service/questionBank/examCollection///BJUI/js/bjui-upload.js HTTP/1.0" 400 968 "http://120.55.144.43/k12platform/service/questionBank/examCollection///BJUI/js/bjui-upload.js" "Mozilla/5.0 (Linux; U; Android 4.4.2; zh-cn; GT-I9500 Build/KOT49H) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 MQQBrowser/5.0 QQ-URL-Manager Mobile Safari/537.36"
100.97.168.32 - - [20/Jun/2016:09:32:25 +0800] "GET /k12platform/service/questionBank/examCollection///BJUI/js/bjui-lookup.js HTTP/1.0" 400 968 "http://120.55.144.43/k12platform/service/questionBank/examCollection///BJUI/js/bjui-lookup.js" "Mozilla/5.0 (Linux; U; Android 4.4.2; zh-cn; GT-I9500 Build/KOT49H) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 MQQBrowser/5.0 QQ-URL-Manager Mobile Safari/537.36"
```


# 查看以`空格`分割的第4位，再以`/`分割的第3位[1天内有效]
`cat access.log | grep "k12platform/service" | awk -F ' ' '{print $4}' | awk -F '/' '{print $3}'`
```
2016:12:50:54

2016:12:50:56
2016:12:50:56
2016:12:50:57
2016:12:50:59
2016:12:51:00
```


# 查看以`空格`分割的第4位，再以`空格`分割的第1位

`cat access.log | grep "k12platform/service" | awk -F ' ' '{print $4}' | awk -F ' ' '{print $1}'`
```
[27/Jun/2016:11:31:56
[27/Jun/2016:12:50:54
[27/Jun/2016:12:50:56
[27/Jun/2016:12:50:56
[27/Jun/2016:12:50:57
[27/Jun/2016:12:50:59
[27/Jun/2016:12:51:00
```


# 查看以`空格`分割的第4位，再以`空格`分割的第1位中第2位开始的数值，且写入到`k12.txt`文件中

`cat access.log | grep "2016" | grep "k12platform/service" | awk -F ' ' '{print $4}' | awk -F ' ' '{print substr($1,2)}' >> k12.txt`
```
18/Jun/2016:11:25:36
18/Jun/2016:11:25:36
18/Jun/2016:11:25:41
18/Jun/2016:11:25:52
18/Jun/2016:11:25:52
18/Jun/2016:11:25:53
18/Jun/2016:11:26:00
18/Jun/2016:11:26:02
18/Jun/2016:11:27:00
```
```
[root@iZ23j23f6aaZ logs]# ll
total 295952
-rw-r--r-- 1 nobody root 274144423 Jun 27 13:10 access.log
-rw-r--r-- 1 nobody root     33422 Jun 27 13:03 error.log
-rw-r--r-- 1 root   root    247527 Jun 27 12:58 k12.txt
-rw-r--r-- 1 root   root         5 Jun 25 15:43 nginx.pid
```


---
`Linux命令大全(手册)_Linux常用命令行实例详解_Linux命令学习手册`
http://man.linuxde.net/


<!-- more -->
