title: linux yum相关
date: 2016-11-23 00:00:02
tags: [linux yum相关]


---


Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。


```
安装软件(以foo-x.x.x.rpm为例）：yum install foo-x.x.x.rpm
yum常用命令
yum常用命令
删除软件：yum remove foo-x.x.x.rpm或者yum erase foo-x.x.x.rpm
升级软件：yum upgrade foo或者yum update foo
查询信息：yum info foo
搜索软件（以包含foo字段为例）：yum search foo
显示软件包依赖关系：yum deplist foo
```
```
　　-e 静默执行
　　-t 忽略错误
　　-R[分钟] 设置等待时间
　　-y 自动应答yes
　　--skip-broken 忽略依赖问题
　　--nogpgcheck 忽略GPG验证
 
　　check-update 检查可更新的包
　　clearn 清除全部
　　clean packages 清除临时包文件（/var/cache/yum 下文件）
　　clearn headers 清除rpm头文件
　　clean oldheaders 清除旧的rpm头文件
　　deplist 列出包的依赖
　　list 可安装和可更新的RPM包
　　list installed 已安装的包
　　list extras 已安装且不在资源库的包
　　info 可安装和可更新的RPM包 信息
　　info installed 已安装包的信息(-qa 参数相似)
　　install[RPM包] 安装包
　　localinstall 安装本地的 RPM包
　　update[RPM包] 更新包
　　upgrade 升级系统
　　search[关键词] 搜索包
　　provides[关键词] 搜索特定包文件名
　　reinstall[RPM包] 重新安装包
　　repolist 显示资源库的配置
　　resolvedep 指定依赖
　　remove[RPM包] 卸载包
```


`yum_百度百科`
http://baike.baidu.com/item/yum


---
```
1.使用YUM查找软件包
命令：yum search
2.列出所有可安装的软件包
命令：yum list
3.列出所有可更新的软件包
命令：yum list updates
4.列出所有已安装的软件包
命令：yum list installed
5.列出所有已安装但不在 Yum Repository 內的软件包
命令：yum list extras
6.列出所指定的软件包
命令：yum list 7.使用YUM获取软件包信息
命令：yum info 8.列出所有软件包的信息
命令：yum info
9.列出所有可更新的软件包信息
命令：yum info updates
10.列出所有已安裝的软件包信息
命令：yum info installed
11.列出所有已安裝但不在 Yum Repository 內的软件包信息
命令：yum info extras
12.列出软件包提供哪些文件
命令：yum provides
```
`linux yum命令详解 - chuncn - 博客园`
http://www.cnblogs.com/chuncn/archive/2010/10/17/1853915.html


# 手册 yum
http://man.linuxde.net/yum
http://www.runoob.com/linux/linux-yum.html