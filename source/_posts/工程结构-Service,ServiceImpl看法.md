title: 工程结构-Service,ServiceImpl看法

date: 2016-07-04 00:00:01
tags: [ 工程结构 ]



---

# 为什么业务实现【Service，ServiceImpl】需要组合出现
- jdk动态代理是面向接口进行的（但CGlib也能支持动态代理常规类）`【非必要条件】`
- 在rpc（服务分布式）的环境中`Service` 作为`能力描述(类似服务发现)` 提供给各需求方。具体的实现`ServiceImpl`保留在当前项目，通过远程调用(socket,rmi,ejb,dubbo)的方式执行具体的实现，再将与`Service方法一致的返回值`响应给调用方(注意对象类型的序列化)。 `【可能条件】`
- 基于rpc的能力描述`Service`思考，模块内业务`Service`是否不必要使用`ServiceImpl`。只需要在对模块间或第三方的远程调用用到的Service陪套ServiceImpl（`或为模块间提供的服务独立编写service+serviceImpl`）    `【预期条件】`


**简述**：在RPC远程访问的场景下，`Service接口`也担任着`能力描述`的功能。思想上`接口和实现松耦合`。`接口提供给客户端`，`实现保留在服务端` （现象的形成，还有很多不同的想法）



---
- webService与以上`Service`的不同使用
webService也是RPC框架，将能力描述`Service`序列号在`wsdl`描述文件中，客户端可通过`wsdl`还原`Service`类，对能力的调用通过`[http + s oap x ml]`进行对服务端的访问，响应的结果依然通过`soap xml`，自行反序列化获得`结构化的对象`。


---
spring实战（第4版）的时候，一种解释:
spring鼓励应用程序的各个层以接口的形式暴露功能，在service层，可以使用service接口+serviceImple实现类，也可以使用service类，但考虑到“接口时实现松耦合的关键”，所以更加推荐使用



` service和serviceImpl的选择 - it馅儿包子 - 博客园 `
http://www.cnblogs.com/zqsky/p/6143319.html


---
#  有多少人在滥用 service+serviceImpl
`2003 年 `  JAVA大概开始流行

` 2004年`  如何使自己的应用或产品既可以跑在mysql上，也可以跑在oracle上
- 这个问题对程序员来说，就是如何编写业务处理层，使之易于移植到其它数据库。



` 2005 年 ` 为解决移植性问题而产生的套路:
- 方式一：把这个Service类设计成一个接口，使控制层只依赖这个接口，于是就有了controller+service+serviceImpl；这样，当某天这个应用要跑在其它数据库上时，就而只需要增加一个serviceImpl类。 `这就是service+serviceImpl套路产生的背景`
- 方式二：service+serviceImpl并非解决这个问题的唯一方案，还有部分项目，他们的团队更有想法，他们把与数据库打交道的代码从service类中提取出来，成为单独的`“数据访问层”（也称为“持久层”）`，于是形成了这样的层次结构    `controller+service+dao+daoImpl`。 了不起！这样对不同的数据库，可以有对应的daoImpl。 相比前面那种方案，而扩展一个daoImpl比扩展一个serviceImpl省事多了。


`2006年`，hibernate从多种`ORMapping`框架中脱颖而出
`2010年`，myBatis诞生，2012年开始流行并讯速得到广泛认可
myBatis的极简用法: myBatis天生就是“依赖反转”、“依赖接口编程”的极佳范本，你无需再弄个daoImpl ```
public interface BasicDao {
    public int save(BasicVo basicVo);
    public int saveBatch(List list);
    public int update(BasicVo basicVo);     ...
```
至此，如需支持多数据库，只需要重写`* Mapper.xml`的sql即可（在基于SQL通用语言下仅需调整部分`关键字,语法`等差异）


**参考**
` 有多少人在滥用 service+serviceImpl，又有多少人在误用myBatis - 林中漫步的个人页面 `
https://my.oschina.net/HuQingmiao/blog/636161

