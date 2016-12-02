title: docker环境CoreOS、CentOS
date: 2016-10-17 00:00:00
tags: [ docker ]



---
- systemd  容器进行管理

-  fleet  容器进行管理， 服务发现和配置信息共享
-  Vagrantfile
-  state partition 状态分区,CoreOS非只读区


---
CoreOS
CoreOS是一种新的、架构体系重新设计的Linux发行版，可以运行在既有的硬件或者云上。CoreOS不提供类似yum或者apt类似的包管理工具，用户不需要在CoreOS中安装软件，而是让程序都在Docker容器中去运行。CoreOS使用systemd和fleet来对容器进行管理，通过etcd进行服务发现和配置信息共享。CoreOS目前风头正劲，目前已经获得融资并于上周宣布收购私有Docker仓库服务商Quay.io，进军企业级的Registry。另外，CoreOS的etcd等组件也获得了社区的认可，并得到了大规模使用。CoreOS已经发布首个稳定版本，目前主流的云服务商都提供了对CoreOS的支持。


`十大基于Docker的开发工具`
http://www.infoq.com/cn/news/2014/08/top-10-open-source-docker/



```
为什么选择 CoreOS?
CoreOS有几个很牛的特点. 它以只读模式运行, 除了“state partition”, 这个地方，我们基本上使用它来存放系统单元文件，包括启动监视你的docker容器, 另外还有docker’的本地图片缓存和一些固定的区域。我喜欢只读文件系统，因为这意味着，他们很稳定，并且几乎不可能被破坏. 另外, CoreOS会自动更新而且它重用了ChromeOS的更新器. 
```
`CoreOS 和 Docker 入门 - 开源中国社区`
http://www.oschina.net/translate/coreos_and_docker_first_steps?cmp


`Docker和CoreOS一直在争夺,为何不互补-阿里云资讯网`
https://www.aliyun.com/zixun/content/1_1_1038419.html


```

最流行最基础的Docker镜像还是“Ubuntu”



老外在这里强烈推荐CoreOS，说怎么怎么好，我还是推荐先使用ubuntu或者在windows上装个vbox在用ubuntu，coreos作为一个新型的服务器级别的系统，也十分值得关注，因为CoreOs将作为主力的docker容器发展。

```
`docker 简明教程 | 简果网`-相关 CoreOS
http://www.simapple.com/docker-tutorial




---
```
完整性、系统性来说CentOS好
CoreOS是一个采用了高度精简的Linux系统内核及外围定制的轻量级操作系统
追问： 那就是说。coreos比centos小
追答： 是的，不过centos也可以最小化安装
追问： 那做服务器他们哪个好点

```
`coreos和centos哪个好_百度知道`

http://zhidao.baidu.com/link?url=dddRjzH1OYs22nMMMt3bG1UlfqVfgY7ldErJaISG56kgXCXlUdMXwdbVjhNtVXifVMWJNZOAbNGnLUnGyHZE8OgasZQ4DB4_E1QrgRvduN_


---
coreos和Ubuntu及centos的本质区别是什么?其实它们本质上都是Linux的独立发行版，大的区别主要有两点：
 
　　1)预装的软件不一样，CoreOS系统预装的软件相比Ubuntu或CentOS少的多(从系统的安装CD内容也能看出来，CoreOS只有 100多MB，Ubuntu的有700多MB)，因此CoreOS系统在启动速度和运行内存上都有优势。另外CoreOS预装的软件很多是为集群环境配备的，例如Etcd和Fleet。
 
　　2)CoreOS的系统分区是只读的，也就说，用户不能直接安装程序到系统里面。所有用户程序的运行都要使用到Docker或Rkt容器，从而确保系统升级时候不会因为系统被用户修改而出现意外问题。
http://www.99inf.com/diannao/rjkf/601619.html 



---
`Docker学习：Coreos+Docker+rancher真方便简捷_服务器应用_Linux公社-Linux系统门户网站`
http://www.linuxidc.com/Linux/2016-04/130605.htm


`借 Docker 东风，CoreOS 蹿红云计算 - 开源中国社区`
http://www.oschina.net/news/55086/coreos-docker

