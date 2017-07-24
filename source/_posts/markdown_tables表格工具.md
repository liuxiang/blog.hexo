title:  markdown_tables表格工具

date: 2017-06-29 00:00:00
tags: [ markdown ]



---

在线： http://www.tablesgenerator.com/markdown_tables


![](http://7xnbs3.com1.z0.glb.clouddn.com/17-7-3/97052676.jpg)
 
| / | 提供client请求目标的方式 | (反)序列化消息体 | 网络服务          | 暴露服务/服务发现 | (反)序列化消息体 | 提供server服务注册方式                                                      | RPC框架初始化 |
|:---------------:|--------------------------|-----------------:|-------------------|-------------------|------------------|-----------------------------------------------------------------------------|---------------|
| Dubbo           | spring-Bean              |    Object-二进制 | tcp(dubbo), nio | zookeeper | 二进制-Object    | spring-Bean                                                                 | spring-Schema |
| webService-Axis | axis.client.Call         | Object-soap(xml) | http              | proName.wsdl描述  | soap(xml)-Object | Apache Axis:deploy.wsdd部署<br>或 JAX-WS (注解) <br> @WebService,@WebMethod | servlet       |
| java.rmi        | Naming.lookup            |    Object-二进制 | tcp(rmi)          | rmi_doc           | 二进制-Object    | LocateRegistry.createRegistry <br> Naming.rebind                             | thread        |
| RESTful         | httpClient               |             json | http              | api_doc           | json             | spring mvc                                                                  | servlet       |
