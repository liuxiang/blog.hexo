title: RPC 相关
date: 2017-6-28 00:00:00
tags: [ RPC ]


---

# RPC
- 远程调用过程
服务消费方（client）以本地调用方式调用远程服务 -`[...]`- server stub根据解码结果调用本地的服务并响应
- 所谓RPC框架即是完成中间过程的过程
【提供client请求目标的方式-(反)序列化消息体-网络客户端】【网络服务端-(反)序列化消息体-调用服务端-提供服务注册方式】


序列化方式：json、xml、hession、protobuf、thrift、text、bytes

![]( http://img0.tuicool.com/qaiMFfA.png!web)


| | | 提供client请求目标的方式 | (反)序列化消息体 | 网络服务 | 暴露服务/服务发现 | (反)序列化消息体 | 提供server服务注册方式 | RPC框架初始化 |
|:---------------:|--------------------------|-----------------|-------------------|-------------------|------------------|---------------------------|---------------|
| Dubbo | spring-Bean |  类方法属性(行为)-> <br> Object<-二进制  | tcp(dubbo), nio | zookeeper |  类方法属性(行为)->反射 <br> 二进制<-Object  | spring-Bean(>ZK) | spring-Schema |
| webService-Axis | axis.client.Call | Object-soap(xml) | http | proName.wsdl描述 | soap(xml)-Object | Apache Axis:deploy.wsdd部署<br>或 JAX-WS (注解)   <br>   @WebService,@WebMethod | servlet |
| java.rmi | Naming.lookup | 类方法属性(行为)-> <br> Object<-二进制  | tcp(rmi) | rmi_doc | 类方法属性(行为)->反射 <br> 二进制<-Object  | LocateRegistry.createRegistry <br> Naming.rebind | thread |
| RESTful | httpClient | json | http | api_doc | json | spring mvc | servlet |


` 你应该知道的RPC原理 - 推酷 `
http://www.tuicool.com/articles/Z3QBVv



---
` 一个虐你千百遍的问题：“RPC好，还是RESTful好？” - 王启军 - 博客频道 - CSDN.NET `
http://blog.csdn.net/douliw/article/details/52592188---

---
# Dubbo
- 节点角色说明
Provider:  暴露服务的服务提供方。
Consumer:  调用远程服务的服务消费方。
Registry:  服务注册与发现的注册中心。
Monitor:  统计服务的调用次调和调用时间的监控中心。
Container:  服务运行容器

- 核心部分
远程通讯: 提供对多种基于长连接的`NIO框架`抽象封装，包括多种线程模型，`序列化`，以及“请求-响应”模式的信息交换方式。
集群容错: 提供基于接口方法的透明远程过程调用，包括多协议支持，以及`软负载均衡`，失败容错，地址路由，动态配置等集群支持。
自动发现: 基于`注册中心`目录服务，使服务消费方能动态的查找服务提供方，使地址透明，使服务提供方可以平滑增加或减少机器。

##  Dubbo 负载均衡策略提供下列四种方式
- Random LoadBalance `随机[ 权重 ]`，按权重设置随机概率。 Dubbo的默认负载均衡策略

在一个截面上碰撞的概率高，但调用量越大分布越均匀，而且按概率使用权重后也比较均匀，有利于动态调整提供者权重。
- RoundRobin LoadBalance `轮循`，按公约后的权重设置轮循比率。

存在慢的提供者累积请求问题，比如：第二台机器很慢，但没挂，当请求调到第二台时就卡在那，久而久之，所有请求都卡在调到第二台上。
- LeastActive LoadBalance `最少活跃调用数`，相同活跃数的随机，活跃数指调用前后计数差。

使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大。
- ConsistentHash LoadBalance `一致性Hash`，相同参数的请求总是发到同一提供者。

当某一台提供者挂时，原本发往该提供者的请求，基于虚拟节点，平摊到其它提供者，不会引起剧烈变动。


` Dubbo负载均衡策略 - 清风部落 - 博客园 `
http://www.cnblogs.com/qingfengbuluo/p/5527930.html

## Dubbo的Spring初始化
> dubbo使用了spring的自定义的Schema完成了dubbo配置的初始化
` dubbo-2.5.3.jar!\META-INF\spring.handlers `
```
http\://code.alibabatech.com/schema/dubbo=com.alibaba.dubbo.config.spring.schema.DubboNamespaceHandler ```
```
public class DubboNamespaceHandler extends NamespaceHandlerSupport {
    public DubboNamespaceHandler() {
}


public void init() {
    this.registerBeanDefinitionParser("application", new DubboBeanDefinitionParser(ApplicationConfig.class, true));
    this.registerBeanDefinitionParser("module", new DubboBeanDefinitionParser(ModuleConfig.class, true));
    this.registerBeanDefinitionParser("registry", new DubboBeanDefinitionParser(RegistryConfig.class, true));
    this.registerBeanDefinitionParser("monitor", new DubboBeanDefinitionParser(MonitorConfig.class, true));
    this.registerBeanDefinitionParser("provider", new DubboBeanDefinitionParser(ProviderConfig.class, true));
    this.registerBeanDefinitionParser("consumer", new DubboBeanDefinitionParser(ConsumerConfig.class, true));
    this.registerBeanDefinitionParser("protocol", new DubboBeanDefinitionParser(ProtocolConfig.class, true));
    this.registerBeanDefinitionParser("service", new DubboBeanDefinitionParser(ServiceBean.class, true));
    this.registerBeanDefinitionParser("reference", new DubboBeanDefinitionParser(ReferenceBean.class, false));
    this.registerBeanDefinitionParser("annotation", new DubboBeanDefinitionParser(AnnotationBean.class, true));
}


static {

    Version.checkDuplicate(DubboNamespaceHandler.class);
}
}
```
```
this.finishRefresh();
this.invokeListener(listener, event);// 对 ApplicationListeners触发 Event  
listener.onApplicationEvent(event);
```
` com.alibaba.dubbo.config.spring.ServiceBean `
```
    public void onApplicationEvent(ApplicationEvent event) {
        if (ContextRefreshedEvent.class.getName().equals(event.getClass().getName()) && this.isDelay() && !this.isExported() && !this.isUnexported()) {
            if (logger.isInfoEnabled()) {
                logger.info("The service ready on spring started. service: " + this.getInterface());
            }
            this.export();
        }
    }
```


` 4. dubbo在spring中的初始代 `
http://blog.csdn.net/u012325403/article/details/55261916


---
# Dubbo 使用示例
- Service
```
    <!-- 提供方应用信息，用于计算依赖关系 -->
    <dubbo:application name="dubbo-server"/>
    <!-- 使用multicast广播注册中心暴露服务地址 -->
    <dubbo:registry address="zookeeper://localhost:2181"/>
    <!-- 用dubbo协议在20880端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880"/>
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.fengjx.dubbo.service.HelloService" ref="helloService"/>
```
```
    <!-- 和本地bean一样实现服务 -->
    <bean id="helloService" class="com.fengjx.dubbo.service.impl.HelloServiceImpl"/>
``` ```
public class HelloServiceImpl implements HelloService {
    /**
     * 暴露的接口
     *
     * @param name
     * @return
     */
    @Override
    public String sayHello(String name) {
        System.out.println("call sayHello");
        return "Hello " + name;
    }
}
```


- Client
```
    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="dubbo-client"/>
    <!-- 使用multicast广播注册中心暴露发现服务地址 -->
    <dubbo:registry address="zookeeper://localhost:2181"/>
    <!-- 生成远程服务代理，可以和本地bean一样使用demoService -->
    <dubbo:reference id="helloService" interface="com.fengjx.dubbo.service.HelloService"/>
```
```
    <bean id="helloServiceConsumer" class="com.fengjx.dubbo.service.HelloServiceConsumer">
        <property name="helloService" ref="helloService"/>
    </bean>
```
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("/spring.xml")
public class HelloServiceConsumerTest {
    @Autowired
    private HelloServiceConsumer helloServiceConsumer;
    @Test
    public void test(){
        String res = helloServiceConsumer.helloFjx();
        System.out.println(res);
        assertEquals("Hello fengjx",res);
    }
}
```

`dubbo接口简单示例，maven + intellj构建`
https://git.oschina.net/fengjx/dubbo-demo.git


`Spring+Dubbo+TestNG接口测试 - LangSand的博客 - CSDN博客 `
http://blog.csdn.net/langsand/article/details/72470720


---
# webService
-  HelloWorld.java [ JAX-WS ]
```
import javax.jws.WebMethod;
import javax.jws.WebService;
import javax.xml.ws.Endpoint;


@WebService(targetNamespace = "http://localhost:9000/HelloWorld")
public class HelloWorld {


@WebMethod
public String sayHello(String name) {
System.out.println(name + ",你好");
return name + ",你好";
}


public static void main(String args[]){
Object hello = new HelloWorld();
String address = "http://localhost:9000/HelloWorld";// 发布到的地址
Endpoint.publish(address, hello);
System.out.println("发布成功,http://localhost:8989/HelloWorld?wsdl");
}
}
```
server依赖
```
<!-- https://mvnrepository.com/artifact/org.apache.axis/axis -->
<dependency>
<groupId>org.apache.axis</groupId>
<artifactId>axis</artifactId>
<version>1.4</version>
</dependency>


<!-- https://mvnrepository.com/artifact/org.apache.axis/axis-jaxrpc -->
<dependency>
<groupId>org.apache.axis</groupId>
<artifactId>axis-jaxrpc</artifactId>
<version>1.4</version>
</dependency>


<!-- https://mvnrepository.com/artifact/axis/axis-wsdl4j -->
<dependency>
<groupId>axis</groupId>
<artifactId>axis-wsdl4j</artifactId>
<version>1.5.1</version>
</dependency>
```
client依赖
```
<!-- https://mvnrepository.com/artifact/commons-logging/commons-logging-api -->
<dependency>
<groupId>commons-logging</groupId>
<artifactId>commons-logging-api</artifactId>
<version>1.1</version>
</dependency>


<!-- https://mvnrepository.com/artifact/commons-discovery/commons-discovery -->
<dependency>
<groupId>commons-discovery</groupId>
<artifactId>commons-discovery</artifactId>
<version>0.5</version>
</dependency>
```


生成`HelloWorld.wsdl` (右键-WebServices`-Generate WSDL Form Java Code`)

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/58767281.jpg)



- 浏览器即可访问(工具:Wizdler)
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/51287937.jpg)


- postman也可访问
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/84860502.jpg)


# 生成客户端
- 方式一： 选择` HelloWorld.wsdl `右键-`Generate Java Code Form Wsdl`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/124167.jpg)


- 方式二：  新建工程（`Apache Axis为web方式`，`JAX-WS为注解方式`）

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/19095284.jpg)


` intellij 开发webservice - 先知后觉 - 博客园`
http://www.cnblogs.com/linxiaoyang/p/4167016.html

` 如何用IDEA一步一步开发WebService服务器端 `
http://blog.csdn.net/u010323023/article/details/52926051
http://www.biliyu.com/article/986.html


` Intellij Idea 下 生成WebServiceClient (WS客户端) - NotFound404 - 博客频道 - CSDN.NET `
http://blog.csdn.net/qq_35067322/article/details/53558775


---
# RMI
` java RMI原理详解 - xinghun_4的专栏 - 博客频道 - CSDN.NET `
http://blog.csdn.net/xinghun_4/article/details/45787549


` Java RMI详解 - 怀揣梦想，努力前行 - 博客频道 - CSDN.NET `
http://blog.csdn.net/a19881029/article/details/9465663


` Java RMI之HelloWorld篇 - 熔 岩 - 51CTO技术博客 `
http://lavasoft.blog.51cto.com/62575/91679/

---
# dubbo 与 WebService区别 跨平台思考?


Dubbo提供基于接口的透明的远程过程调用，支持`多种协议`，包括Dubbo、RMI、`WebService`、Hessian、Http、Thrift、Redis、Memcached。
Dubbo基于注册中心的目录服务，使服务消费方能动态的查找服务提供方，支持平滑的减少或增加服务器。

` 分布式服务框架Dubbo入门实践 - 简书 `
http://www.jianshu.com/p/d357dada2ff5



---
# EJB
` EJB分布式工作原理 - Senssic - 博客频道 - CSDN.NET `
http://blog.csdn.net/senssic/article/details/11850131


` EJB 工作原理 - 企业应用 - Java - ITeye论坛 `
http://www.iteye.com/topic/3832


` EJB工作原理学习笔记 - 天下无贼 - 51CTO技术博客 `
http://guojuanjun.blog.51cto.com/277646/1351245

` EJB工作原理学习笔记! - 李晓辉的Blog - 博客频道 - CSDN.NET `
http://blog.csdn.net/tolixiaohui/article/details/211957


- ` Weblogic `
WebLogic是美国Oracle公司出品的一个application server，确切的说是一个基于JAVAEE架构的中间件 同类有:tomcat


- ` XDoclet`
    XDoclet 是一个通用的代码生成实用程序,是一个扩展的Javadoc Doclet引擎，它允许您使用象 JavaDoc 标记之类的东西来向诸如类、方法和字段之类的语言特征添加元数据
    XDoclet 继承了 JavaDoc 引擎的思想，允许根据定制 JavaDoc 标记生成代码和其他文件。当然，XDoclet 也可以访问整个解析树。这样，它就可以访问类、类的包结构和类的方法。

` XDoclet是什么? - yangjuqi的日志 - 网易博客 `

http://blog.163.com/yangjuqi@126/blog/static/28452285200791192152923/



---
**参考**
`Dubbo初探 - 推酷`
http://www.tuicool.com/articles/VrQrUv


` Dubbo源码学习--集群负载均衡算法的实现 `
http://www.cnblogs.com/javanoob/p/dubbo_loadbalance.html


`RPC框架与Dubbo完整使用`
http://blog.csdn.net/u010297957/article/details/51702076

` Java RMI 框架（远程方法调用） - 蚂蚁 - 51CTO技术博客 `
http://haolloyin.blog.51cto.com/1177454/332426/




