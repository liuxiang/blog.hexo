title: Java程序,服务保障 
date: 2015-10-06 00:00:00 #发表日期，一般不改动
categories: 监测维护 #文章文类
tags: [java,服务器监测,问题定位分析] #文章标签，多于一项时用这种格式
photos:
- http://7xnbs3.com1.z0.glb.clouddn.com/15-10-8/82324295.jpg
- http://7xnbs3.com1.z0.glb.clouddn.com/15-10-8/10907762.jpg
- http://7xnbs3.com1.z0.glb.clouddn.com/15-10-19/15047487.jpg
---


综合能力工具支持:
健康检查，监控，性能瓶颈分析，问题定位，轨迹，告警




## JDK 自带 性能分析&健康检查 工具箱-能力：
```
1、  显示虚拟机的进程以及进程的配置信息和环境信息(jps、jinfo)
2、  监视应用程序的CPU、内存、堆、方法区和线程信息(jstat、jstack)
3、  Dump以及分析dump的功能(jmap、jhat)
4、  离线程序快照：离线dump分析
5、  方法运行性能分析，找出调用最多，运行最长的方法块
```


## JDK 工具箱，可视化工具集： jconsole ， VisualVM ， JMC
相比命令工具，增加可视化，监控，轨迹收集统计


## java 定位分析工具
```
samurai.jar                           分析线程堆栈，故障当前可找出问题程序的具体代码行。
gcviewer-1.35-SNAPSHOT.jar            分析jvm GC轨迹，可预警内存泄露。
MemoryAnalyzer                        分析内存Dump，可分析内存中具体的对象。可排查内存泄露，溢出。
```


<!-- more -->


## 阿里云监控（仅对阿里云产品有效）
监控目标是机器而不是JVM，其次可对高位资源使用情况发出告警（短信&邮箱）
![阿里云监控](http://7xnbs3.com1.z0.glb.clouddn.com/15-10-8/82324295.jpg "阿里云监控")


## 专业级分析工具，JProfiler:
可以查看当前应用的对象、对象引用、内存、CPU使用情况、线程、线程运行情况（阻塞、等待等），同时可以查找应用内存使用得热点.
![JProfiler 内存对象](http://7xnbs3.com1.z0.glb.clouddn.com/15-10-19/15047487.jpg)
![JProfiler 线程内存&耗时](http://7xnbs3.com1.z0.glb.clouddn.com/15-10-8/10907762.jpg)


