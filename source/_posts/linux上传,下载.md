title: linux上传,下载
date: 2017-1-24 00:00:02
tags: [ linux ]


---

#工具
`lrzsz`



#  下载&安装
```
wget https://ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz
tar zxvf lrzsz-0.12.20.tar.gz && cd lrzsz-0.12.20
./configure && make && make install
```
上面安装过程默认把lsz和lrz安装到了/usr/local/bin/目录下，现在我们并不能直接使用，下面创建软链接，并命名为rz/sz：
```
cd /usr/bin
ln -s /usr/local/bin/lrz rz
ln -s /usr/local/bin/lsz sz
```


# 使用说明
sz命令发送文件到本地：`sz filename` 配合文件夹压缩 ` tar zcvf filaname.tar.gz filename`
rz命令本地上传文件到服务器：`rz` (上传)  `rz -y`(覆盖上传)
(执行该命令后，在弹出框中选择要上传的文件即可)


---
# 配套相关
- 压缩
 tar cvf lk.tar.gz lk

-  解压
unzip -d iserver -o iserver.zip

