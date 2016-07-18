title: JVM参数配置
date: 2015-10-07 03:00:00 #发表日期，一般不改动
categories: coding #文章文类
tags: [java,JVM] #文章标签，多于一项时用这种格式


---


## 常见配置
```vala
# 4G内存:
JAVA_OPTS="$JAVA_OPTS -server -Xms2500m -Xmx2500m -XX:PermSize=256M -XX:MaxPermSize=512m"
```


```vala
# 机器内存分配结构：
#    核心应用占用内存（可通过监控查看出合适的大小，配置太高用不到也是浪费）
#    系统及非核心应用占用内存（在核心应用未启用时，top所占用内存。此基础上还需弹性空间）
#    注:日常状态内存尽量控制在70%以内,预留内存以备处理‘内存针刺’的业务
# (服务器一般设置-Xms、-Xmx相等以避免在每次GC后调整堆的大小)


-Xms128m                #JVM初始分配的堆内存
-Xmx512m                #JVM最大允许分配的堆内存，按需分配
-XX:PermSize=64M        #JVM初始分配的非堆内存
-XX:MaxPermSize=128M    #JVM最大允许分配的非堆内存，按需分配
```
<!-- more -->


## tomcat jvm配置
```vala
# 服务端配置远程调试
CATALINA_OPTS="$CATALINA_OPTS -server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5888"


# jconsole，VisualVM，JMC 监控配置（rmi）
# -Dcom.sun.management.jmxremote.authenticate=false 为 JMC 关闭飞行模式
JAVA_OPTS="$JAVA_OPTS -Dcom.sun.management.jmxremote=true -Djava.rmi.server.hostname=121.40.84.8 -Dcom.sun.management.jmxremote.port=8999 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.managementote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false"


# 配置tomcat，日常GC日志. 可记录下服务器历史的GC情况
# 注意：tomcat启动后如果webapps下未自动创建sys目录，请手动创建 */webapps>mkdir sys
# 配置路径到webapps下，方便日志文件的下载和直接定位：
# 访问：
# http://121.40.87.121:8080/sys/gc.log
# http://121.40.84.8:8080/sys/gc.log
# 可视化工具:GCViewer可以使用网络地址直接查看
CATALINA_OPTS="$CATALINA_OPTS -Xloggc:webapps/sys/gc.log -XX:+PrintGCDetails -XX:+PrintGCDateStamps"


# jprofile服务器端配置
CATALINA_OPTS="$CATALINA_OPTS -agentlib:jprofilerti=port=8849,nowait -Xbootclasspath/a:/usr/local/jprofile/jprofiler9/bin/agent.jar"


# jvm shut down时,输出dump文件
CATALINA_OPTS="$CATALINA_OPTS -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/usr/local/apache-tomcat-7.0.59/logs/heap.dump"
```


windows 配置
```vala
# 服务端配置远程调试
SET CATALINA_OPTS=-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5888
 
# jprofile服务器端配置
set CATALINA_OPTS=%CATALINA_OPTS% -agentpath:G:\workTool\jprofiler9\bin\windows-x64\jprofilerti.dll=port=8849,nowait
```



