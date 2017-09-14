title: 【java脚手架搭建】注解 spring-context-annotation.xml
date: 2017-3-8 00:00:05
tags: [ 脚手架 ]


---


# 注解相关
`@Service` 用于标注业务层组件
`@Controller` 用于标注控制层组件（如struts中的action）
`@Repository` 用于标注数据访问组件，即DAO组件
`@Component` 泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。


- 初始化
`<context:component-scan`标签默认情况下自动扫描指定路径下的包（含所有子包），将带有
`@Component、@Repository、@Service、@Controller`
标签的类自动注册到spring容器。
对标记了 
`Spring's` @Required、@Autowired、
`JSR250's` @PostConstruct、@PreDestroy、@Resource、
`JAX-WS's` @WebServiceRef、
`EJB3's` @EJB、
`JPA's` @PersistenceContext、@PersistenceUnit
等注解的类进行对应的操作使注解生效（包含了annotation-config标签的作用）。


- 自动注入
```
@Autowired后不需要getter()和setter()方法，Spring也会自动注入
当接口存在两个实现类的时候必须使用@Qualifier指定注入哪个实现类，否则可以省略，只写@Autowired
```


**参考**
`Spring注解@Component、@Repository、@Service、@Controller区别 - zhang854429783的专栏`
http://blog.csdn.net/zhang854429783/article/details/6785574


---
# 注解配置
- 支持mvc注解?
```xml
<mvc:annotation-driven>
```


- 启用spring mvc 注解?
```xml
<!-- 启用spring mvc 注解 -->
<context:annotation-config />
```


- 注解获取配置值
```xml
<!-- 加载应用属性实例，可通过  @Value("#{config['jdbc.driver']}") String jdbcDriver 方式引用 -->
<util:properties id="config" location="classpath:config.properties" local-override="true"/>
```


- 自动Bean注册`Annotation`
```xml
`spring-context.xml`
<!-- `@Service`注解生效 -->
<context:component-scan base-package="com.wosai.bright.service"/>
```
`spring-servlet.xml`
```xml
<!-- `@RequestMapping`注解生效(servlet映射) -->
<context:component-scan base-package="com.wosai.bright.controller"/>
```
或
`spring-context.xml`
```xml
<!-- 使用Annotation自动注册Bean，解决事物失效问题：在主容器中不扫描@Controller注解，在SpringMvc中只扫描@Controller注解。  -->
<context:component-scan base-package="com.thinkgem.jeesite"><!-- base-package 如果多个，用“,”分隔 -->
<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```
`spring-mvc.xml`
```xml
<!-- 使用Annotation自动注册Bean,只扫描@Controller -->
<context:component-scan base-package="com.thinkgem.jeesite" use-default-filters="false"><!-- base-package 如果多个，用“,”分隔 -->
<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
````
- 支持事务注解的（`@Transactional`） 
```xml
<tx:annotation-driven/>
``` 
- `@Transactional`注解的类定义事务
```xml
<!-- 配置 Annotation 驱动，扫描@Transactional注解的类定义事务  -->
<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
```


- `@Aspect`注解代理(aspect切面)
```xml
<!-- 激活自动代理功能:支持class代理(cglib) -->
<aop:aspectj-autoproxy proxy-target-class="true" />
```
`proxy-target-class 作用 - Leo's Blog - CSDN博客`
http://blog.csdn.net/lc0817/article/details/46580735


更多
```xml
<aop:advisor> 定义一个AOP通知者 
<aop:after> 后通知 
<aop:after-returning> 返回后通知 
<aop:after-throwing> 抛出后通知 
<aop:around> 周围通知 
<aop:aspect>定义一个切面 
<aop:before>前通知 
<aop:config>顶级配置元素，类似于<beans>这种东西 
<aop:pointcut>定义一个切点 
```


- 注解：计划任务
```xml
<!-- 计划任务配置，用 @Service @Lazy(false)标注类，用@Scheduled(cron = "0 0 2 * * ?")标注方法 -->
<task:executor id="executor" pool-size="10"/>
<task:scheduler id="scheduler" pool-size="10"/>
<task:annotation-driven scheduler="scheduler" executor="executor" proxy-target-class="true"/>
```


---
# MVC注解支持 & 数据处理
```xml
<!--RequestMappingHandlerAdapter-->
<mvc:annotation-driven>
  <mvc:message-converters>
    <ref bean="stringHttpMessageConverter"/>
    <ref bean="marshallingHttpMessageConverter"/>
    <ref bean="mappingJackson2HttpMessageConverter"/>
  </mvc:message-converters>
</mvc:annotation-driven>


<context:component-scan base-package="com.wosai.framework.controller"/>


<bean id="stringHttpMessageConverter"
      class="org.springframework.http.converter.StringHttpMessageConverter"/>


<bean id="marshallingHttpMessageConverter"
      class="org.springframework.http.converter.xml.MarshallingHttpMessageConverter">
  <property name="marshaller" ref="castorMarshaller"/>
  <property name="unmarshaller" ref="castorMarshaller"/>
</bean>


<bean id="mappingJackson2HttpMessageConverter"
      class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
  <property name="supportedMediaTypes">
    <list>
      <value>application/json</value>
      <value>application/xml</value>
      <value>text/html</value>
      <value>text/plain</value>
      <value>text/xml</value>
    </list>
  </property>
</bean>


<bean id="castorMarshaller" class="org.springframework.oxm.castor.CastorMarshaller"/>
```


`<mvc:annotation-driven>` 标签，controller类相关注解（`@RequestMapping`,`@Controller`）及url映射支持
`<mvc:message-converters>` 标签，设定字符集和json处理类

