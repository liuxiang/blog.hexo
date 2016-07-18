


title: jdk工具命令——jstack查看系统线程堆栈
date: 2012-2-30 00:00:00
categories: jdk工具命令
tags: [jdk工具命令，jstack]


---




# 查看当前 Java 系统线程执行状况




## 使用场景

* 1. 定位环境慢 / 功能 慢问题 . 在慢的过程中将执行的堆栈信息 已规律 的间隔 (5~10 秒 ) 打印出来 . 在连续几次都出现的线程一般是有问题的线程 , 因为在执行 10~20 秒中都未销毁的线程 .

* 2. 系统 CPU 高居不下 .

堆栈观察 : 首先找到平台产生的进程 ( Ctrl+F:com.\***.\** ), 记录线程 ID. 找出连续几次堆栈打印都出现的线程 , 分析是否可疑 .




## 操作

* iread190 /home/manager> ` jps`

```

25826 Jps       ( 此为 jps 命令的进程 )

9154 Bootstrap ( 此为 java 进程 )

```




* iread190 /home/manager> ` jstack 9154`

```

"RMI TCP Accept-0" daemon prio =10 tid =0x00002aaab089d000 nid =0x23d9 runnable [0x000000004163e000]

   java.lang.Thread.State : RUNNABLE

at java.net.PlainSocketImpl.socketAccept (Native Method)

at java.net.PlainSocketImpl.accept (PlainSocketImpl.java:408)

        - locked <0x00000006f9c00580> (a java.net.SocksSocketImpl )

at java.net.ServerSocket.implAccept (ServerSocket.java:462)

at java.net.ServerSocket.accept (ServerSocket.java:430)

at sun.management.jmxremote.LocalRMIServerSocketFactory$1.accept(LocalRMIServerSocketFactory.java:34)

at sun.rmi.transport.tcp.TCPTransport$AcceptLoop.executeAcceptLoop(TCPTransport.java:369)

at sun.rmi.transport.tcp.TCPTransport$AcceptLoop.run(TCPTransport.java:341)

at java.lang.Thread.run (Thread.java:662)

 

"RMI TCP Accept-29011" daemon prio =10 tid =0x00002aaab0899000 nid =0x23d8 runnable [0x000000004153d000]

   java.lang.Thread.State : RUNNABLE

at java.net.PlainSocketImpl.socketAccept (Native Method)

at java.net.PlainSocketImpl.accept (PlainSocketImpl.java:408)

        - locked <0x00000006f9c008a0> (a java.net.SocksSocketImpl )

at java.net.ServerSocket.implAccept (ServerSocket.java:462)

at java.net.ServerSocket.accept (ServerSocket.java:430)

at sun.rmi.transport.tcp.TCPTransport$AcceptLoop.executeAcceptLoop(TCPTransport.java:369)

at sun.rmi.transport.tcp.TCPTransport$AcceptLoop.run(TCPTransport.java:341)

at java.lang.Thread.run (Thread.java:662)

 

……

```

 

# 将堆栈写入当前目录下文件(相隔几秒多次执行) :

* 获取5s*4的时间内，都没有结束的业务线程，多见为问题线程，需要看看其中在执行什么

```

jstack 9154 >> jstack.log

<相隔5s>



jstack 9154 >> jstack.log

<相隔5s>

jstack  9154 >> jstack.log

<相隔5s>
jstack  9154 >> jstack.log

```




# 怎么分析

`jstackAnalyse@liuxiang` 自己编写的堆栈日志分析工具


`samurai.jar` 分析线程堆栈，故障当前可找出问题程序的具体代码行。


---


**扩展阅读**
`java服务器资源（CPU，内存）使用率过高时，定位处理过程`


<!-- more -->

