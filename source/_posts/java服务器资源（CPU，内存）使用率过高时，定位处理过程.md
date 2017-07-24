title: java服务器资源（CPU，内存）使用率过高时，定位处理过程
date: 2015-10-19 00:00:00 #发表日期，一般不改动
categories: 监测维护 #文章文类
tags: [java,服务器监测,问题定位分析] #文章标签，多于一项时用这种格式
photos:
- http://7xnbs3.com1.z0.glb.clouddn.com/15-10-19/69991098.jpg
- http://yusuke.homeip.net/samurai/en/images/threadTab_en.gif
- http://7xnbs3.com1.z0.glb.clouddn.com/15-10-8/10907762.jpg
- http://7xnbs3.com1.z0.glb.clouddn.com/15-10-19/15047487.jpg
---


# 找出问题进程
`top`  服务器内存 / `top -H -p <进程ID>`
`jps` 获取当前java进程号PID


# 对象内存分布
`jmap -histo:live [pid]`


```
# 正则提取：.*com.wosai.*
 322:           161          11592  com.wosai.uncle.entity.Program
 600:            64           3072  com.wosai.uncle.entity.SeckillActivity
 711:            37           2072  com.wosai.uncle.entity.Video
 758:            21           1680  com.wosai.uncle.entity.WeixinUser
 812:            20           1440  com.wosai.uncle.entity.SignUp
 843:            20           1280  com.wosai.uncle.entity.Register
 855:            17           1224  com.wosai.uncle.entity.SeckillActivityGoods
 863:            25           1200  com.wosai.uncle.entity.Activity
 884:            20           1120  com.wosai.uncle.entity.Message
 937:            20            960  com.wosai.uncle.entity.User
1066:            20            640  com.wosai.uncle.dto.MessageDto
1126:            10            560  com.wosai.uncle.entity.SeckillUser
1168:             9            504  com.wosai.uncle.entity.OrderProduct
1171:            20            480  com.wosai.uncle.dto.RegisterDto
1173:            20            480  com.wosai.uncle.dto.VideoProgramDto
1179:            20            480  com.wosai.uncle.dto.SignUpDto
1339:             6            288  com.wosai.uncle.entity.Host
1368:             7            280  com.wosai.uncle.entity.Channel
```


<!-- more -->
# 查看GC回收情况(预先配置VM -Xloggc:gc.log)
http://ip/sys/gc.log
![gcviewer-1.35-SNAPSHOT.jar](http://7xnbs3.com1.z0.glb.clouddn.com/15-10-19/69991098.jpg)


## 手动收集当前GC情况
*   `jstat -gc [pid] 1000 5`  每隔1秒监控一次，5次


```
[root@iZ23ka2dtiqZ ~]# jstat -gc 24094 1000 5
 S0C    S1C    S0U    S1U      EC       EU        OC         OU       PC     PU    YGC     


YGCT    FGC    FGCT     GCT   
76800.0 2560.0  0.0   2330.4 702976.0 453772.8 1707008.0  1397535.0  262144.0 102459.2   


1087  129.358   0      0.000  129.358
76800.0 2560.0  0.0   2330.4 702976.0 455408.6 1707008.0  1397535.0  262144.0 102459.2   


1087  129.358   0      0.000  129.358
76800.0 2560.0  0.0   2330.4 702976.0 457124.6 1707008.0  1397535.0  262144.0 102459.2   


1087  129.358   0      0.000  129.358
76800.0 2560.0  0.0   2330.4 702976.0 458698.6 1707008.0  1397535.0  262144.0 102459.2   


1087  129.358   0      0.000  129.358
76800.0 2560.0  0.0   2330.4 702976.0 460545.1 1707008.0  1397535.0  262144.0 102459.2   


1087  129.358   0      0.000  129.358
```
* `jstat -gcutil [pid] 1000 10` 按百分比显式


```
[root@iZ23ka2dtiqZ ~]# jstat -gcutil 24094 1000 10
  S0     S1     E      O      P     YGC     YGCT    FGC    FGCT     GCT   
  0.00  91.03  45.76  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  46.05  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  46.25  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  46.55  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  46.78  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  47.02  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  47.30  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  47.54  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  47.76  81.87  39.08   1087  129.358     0    0.000  129.358
  0.00  91.03  47.81  81.87  39.08   1087  129.358     0    0.000  129.358
```


# 收集（多次）内存堆栈信息. 分析工具：samurai.zip
假设多次收集，时间跨度30s,依然存在的平台线程即说明在此周期内都未曾执行结束，就很可能有问题（排查守候线程）
自编写线程堆栈分析神器：jstackAnalyse_wosai.java
`kill -3 [pid]` or `jstack [PID] >> jstack.log`



![samurai](http://yusuke.homeip.net/samurai/en/images/threadTab_en.gif)




# 最后：镜像内存dump(较大,且在服务器上) . 查看工具:IBM HeapAnalyzer (MAT)
`jmap -dump:format=b,file=dumpfileName.dump [PID]`


---
# 先找高消耗（内存/cpu）进程->再找高消耗（内存/cpu）线程->记录线程pid的十进制->找到内存堆栈中的具体执行内容
（纪念-问题发生在2011-7月）
```
>top
  PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
7765 manager 16 0 2246m 1.5g 11m S 248 9.6 577:45.56 java
1705 oracle 15 0 1714m 62m 57m S 28 0.4 0:02.35 oracle
32217 oracle 15 0 1713m 227m 223m S 4 1.4 1:43.31 oracle
10599 oracle 16 0 1715m 541m 536m R 4 3.4 26:42.11 oracle
7896 oracle 16 0 1714m 528m 523m S 3 3.3 30:01.47 oracle
 
>top -H -p [进程ID]
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
32196 portal 18 0 3197m 462m 9492 S 999999 5.8 0:00.00 java
32187 portal 16 0 3197m 462m 9492 S 989898 5.8 0:10.25 java
32188 portal 16 0 3197m 462m 9492 S 0 5.8 0:00.49 java
20378 portal 16 0 3197m 462m 9492 S 0 5.8 0:00.41 java
 
>使用计算器将十进制转换为十六进制: 32196 -> 7DC4
 
>kill -3 <进程ID>  (或：jstack [PID] >> jstack.log)
 
>将日志下载至本地，这时就可以使用原记录的十六进制线程ID了，文件搜索线程ID，观察处理逻辑。
":PlanEngineWorker-702" prio=10 tid=0x00002aab1ac06c00 nid=0x7dc4 waiting for monitor entry [0x00000
0005173d000..0x000000005173ee20]
```
---
# 监控同步,相关工具  jconsole ， VisualVM ， JMC， Jprofile
![JProfiler 内存对象](http://7xnbs3.com1.z0.glb.clouddn.com/15-10-19/15047487.jpg)
![JProfiler 线程内存&耗时](http://7xnbs3.com1.z0.glb.clouddn.com/15-10-8/10907762.jpg)
