title: linux 1.新磁盘格式化->2.mount挂载->3.软连接ln
date: 2016-08-10 00:00:00
tags: [ linux ]


---


# 一.磁盘格式化
## 未格式化状态`Disk identifier: 0x00000000`
```
[root@iZ23ult4m4yZ ~]# fdisk -l
 
Disk /dev/xvda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00078f9c
 
    Device Boot      Start         End      Blocks   Id  System
/dev/xvda1   *           1        2611    20970496   83  Linux
 
Disk /dev/xvdb: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000
```


# 格式化
-   运行 fdisk /dev/xvdb，对数据盘进行分区。根据提示，依次输入 n，p，1，两次回车，wq，分区就开始了

![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/82644598.jpg)


-   运行 fdisk -l 命令，查看新的分区。新分区 xvdb1 已经创建好。如下面示例中的/dev/xvdb1。
![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/39930823.jpg)


-   运行 mkfs.ext3 /dev/xvdb1，对新分区进行格式化。格式化所需时间取决于数据盘大小。您也可自主决定选用其他文件格式，如 ext14 等。

![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/35756625.jpg)


-   运行 echo /dev/xvdb1 /mnt ext3 defaults 0 0 >> /etc/fstab 写入新分区信息。完成后，可以使用 cat /etc/fstab 命令查看。

![]( http://7xnbs3.com1.z0.glb.clouddn.com/16-9-24/20102328.jpg)


# 二.mount挂载

- 运行 mount /dev/xvdb1 /mnt 挂载新分区，然后执行 df -h 查看分区。如果出现数据盘信息，说明挂载成功，可以使用新分区了。
```
mount /dev/xvdb1 /mnt
df -h
 Filesystem      Size  Used Avail Use% Mounted on
 /dev/xvda1       40G  1.5G   36G   4% /
 tmpfs           498M     0  498M   0% /dev/shm
 /dev/xvdb1      5.0G  139M  4.6G   3% /mnt
```


# 三.软连接ln



- 创建`/mnt/nginx`，待转移目录
```
mkdir /mnt/nginx
```


- 转移文件`/usr/local/nginx/logs`
```
mv  /usr/local/nginx/logs /mnt/nginx/
```

- 建立软连接`ln -s`( /usr/local/nginx/logs目录会自动创建 )，查看
```
ln -s /mnt/nginx/logs/ /usr/local/nginx/logs


ll  /usr/local/nginx/
```


- 可能遇到的疑问：mv转移文件后`df -h`统计并没有减少,是因为文件被提前占用磁盘并没有释放.
     - `lsof |grep deleted`可查看
```
[root@iZ23ult4m4yZ logs]#  lsof |grep deleted
java        394     root  112u      REG              202,1          0     409568 /usr/local/apache-tomcat-7.0.59/temp/org.hibernate.cache.spi.UpdateTimestampsCache.data (deleted)
java        394     root  113u      REG              202,1          0     409684 /usr/local/apache-tomcat-7.0.59/temp/org.hibernate.cache.internal.StandardQueryCache.data (deleted)
mingetty   1034     root  txt       REG              202,1      15256     655433 /sbin/mingetty (deleted)
mingetty   1036     root  txt       REG              202,1      15256     655433 /sbin/mingetty (deleted)
mingetty   1038     root  txt       REG              202,1      15256     655433 /sbin/mingetty (deleted)
mingetty   1043     root  txt       REG              202,1      15256     655433 /sbin/mingetty (deleted)
mingetty   1045     root  txt       REG              202,1      15256     655433 /sbin/mingetty (deleted)
AliHids   19218     root   24uW     REG              202,1          0     396342 /tmp/qtsingleapp-aegisG-46d2-lockfile (deleted)
```
回收办法，重启占用具体文件的进程
```
nginx -t stop  # 停止服务。或 kill <pid> , ps -ef|grep nginx 取得<pid>  
nginx # 启动
df -h # 重新查看磁盘占用情况
```


---


`云服务器 ECS_快速入门(Linux)_步骤 4：格式化和挂载数据盘-阿里云产品文档`
https://help.aliyun.com/document_detail/25426.html


`阿里云ECS挂载磁盘 - Tony打豆豆 - SegmentFault`
https://segmentfault.com/a/1190000004692786


`[原]linux删除文件后没有释放空间 - 冰刀(skate) - 博客频道 - CSDN.NET`
http://blog.csdn.net/wyzxg/article/details/4971843

