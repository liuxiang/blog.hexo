title: 【java脚手架搭建】问题处理
date: 2017-3-9 00:00:00
tags: [ 脚手架 ]


---


# 无法访问到控制器
- 日志
```text
2017-03-09 10:07:13,420 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Creating shared instance of singleton bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping'
2017-03-09 10:07:13,420 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Creating instance of bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping'
2017-03-09 10:07:13,426 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Eagerly caching bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping' to allow for resolving potential circular references
2017-03-09 10:07:13,449 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Returning cached instance of singleton bean 'mvcContentNegotiationManager'
2017-03-09 10:07:13,455 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Creating shared instance of singleton bean 'org.springframework.web.servlet.handler.MappedInterceptor#0'
2017-03-09 10:07:13,455 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Creating instance of bean 'org.springframework.web.servlet.handler.MappedInterceptor#0'
2017-03-09 10:07:13,648 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Invoking afterPropertiesSet() on bean with name 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping'
2017-03-09 10:07:13,648 DEBUG [servlet.mvc.method.annotation.RequestMappingHandlerMapping] - Looking for request mappings in application context: WebApplicationContext for namespace 'springServlet-servlet': startup date [Thu Mar 09 10:07:13 CST 2017]; parent: Root WebApplicationContext


2017-03-09 10:07:13,661 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Finished creating instance of bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping'
```
`其中缺少了mapping登记`,见下文


- 正常`@RequestMapping`初始日志
``` text
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Creating shared instance of singleton bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping#0'
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Creating instance of bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping#0'
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Eagerly caching bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping#0' to allow for resolving potential circular references
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Returning cached instance of singleton bean 'mvcContentNegotiationManager'
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Returning cached instance of singleton bean 'org.springframework.web.servlet.handler.MappedInterceptor#0'
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Invoking afterPropertiesSet() on bean with name 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping#0'
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Looking for request mappings in application context: WebApplicationContext for namespace 'mybatis-servlet': startup date [Thu Mar 09 10:16:44 CST 2017]; parent: Root WebApplicationContext
 INFO [RMI TCP Connection(5)-127.0.0.1] - Mapped "{[/delete],methods=[],params=[],headers=[],consumes=[],produces=[],custom=[]}" onto public java.lang.String com.isea533.mybatis.controller.demo.CountryController.delete(java.lang.Integer)
 INFO [RMI TCP Connection(5)-127.0.0.1] - Mapped "{[/save],methods=[POST],params=[],headers=[],consumes=[],produces=[],custom=[]}" onto public org.springframework.web.servlet.ModelAndView com.isea533.mybatis.controller.demo.CountryController.save(com.isea533.mybatis.model.Country)
 INFO [RMI TCP Connection(5)-127.0.0.1] - Mapped "{[/list || /index || /index.html || ],methods=[],params=[],headers=[],consumes=[],produces=[],custom=[]}" onto public org.springframework.web.servlet.ModelAndView com.isea533.mybatis.controller.demo.CountryController.getList(com.isea533.mybatis.model.Country,int,int)
 INFO [RMI TCP Connection(5)-127.0.0.1] - Mapped "{[/view],methods=[GET],params=[],headers=[],consumes=[],produces=[],custom=[]}" onto public org.springframework.web.servlet.ModelAndView com.isea533.mybatis.controller.demo.CountryController.view(com.isea533.mybatis.model.Country)
DEBUG [RMI TCP Connection(5)-127.0.0.1] - Finished creating instance of bean 'org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping#0'
```
- 解决办法:在`spring-servlet.xml`增加 `@ RequestMapping ` 注解支持.解决
```xml
<!-- `@RequestMapping`注解生效(servlet映射) -->
<context:component-scan base-package="com.wosai.bright.controller"/>
```


---
# Context initialization failed 上下文初始化异常
`countryController`类中`CountryService`属性`@Autowired`注解装配失败.
``` text
2017-03-09 10:48:23,581 ERROR [org.springframework.web.servlet.DispatcherServlet] - Context initialization failed


org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'countryController': Injection of autowired dependencies failed; 


nested exception is org.springframework.beans.factory.BeanCreationException: Could not autowire field: private com.wosai.bright.service.CountryService com.wosai.bright.controller.CountryController.countryService; 


nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [com.wosai.bright.service.CountryService] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
```
- 解决办法： 在`spring-content.xml`增加 `@Service` 注解支持.解决
```xml
<!-- `@Service`注解生效 -->
<context:component-scan base-package="com.wosai.bright.service"/>
```


---
# 无法获取到表名 & 异常返回至`error/500`
``` text
2017-03-09 11:19:11,769 DEBUG [springframework.web.servlet.handler.SimpleMappingExceptionResolver] - Resolving to view 'error/500' for exception of type [tk.mybatis.mapper.MapperException], based on exception mapping [java.lang.Throwable]
2017-03-09 11:19:11,769 DEBUG [springframework.web.servlet.handler.SimpleMappingExceptionResolver] - Exposing Exception as model attribute 'exception'
2017-03-09 11:19:11,770 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Handler execution resulted in exception - forwarding to resolved error view: ModelAndView: reference to view with name 'error/500'; model is {exception=tk.mybatis.mapper.MapperException: 无法获取实体类com.wosai.bright.model.Country对应的表名!}


tk.mybatis.mapper.MapperException: 无法获取实体类com.wosai.bright.model.Country对应的表名!
```
异常控制
```xml
<!-- 异常控制 -->
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
<property name="exceptionMappings">
<props>
<prop key="org.apache.shiro.authz.UnauthorizedException">error/403</prop>
<prop key="java.lang.Throwable">error/500</prop>
</props>
</property>
</bean>
```
- 解决办法: 配置`Mapper<Entity>类`支持
```xml
<!-- @MyBatisDao注解的接口(Mapper<Entity>类支持) -->
<bean id="mapperScannerConfigurer" class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="com.wosai.bright.mapper"/>
<!-- 3.2.2版本新特性，markerInterface可以起到mappers配置的作用，详细情况需要看Marker接口类 -->
<property name="markerInterface" value="com.wosai.bright.common.MyMapper"/>
<!-- 通用Mapper通过属性注入进行配置，默认不配置时会注册Mapper<T>接口-->
<property name="properties">
<value>
mappers=tk.mybatis.mapper.common.Mapper
</value>
</property>
</bean>
```
**参考**
[`#158 Example(Entity.class)在@Controller类里面作为参数构造的时候会报错"无法获取实体类xxx对应的表名!"`](http://git.oschina.net/free/Mapper/issues/158)
[` #143 example 设置为全局变量的时候报错“无法获取实体类xxx对应的表名”`]( http://git.oschina.net/free/Mapper/issues/143)


---
# 数据库查询异常,` bright.country ` 表 不存在
` Table 'bright.country' doesn't exist  `
``` text
org.springframework.web.util.NestedServletException: Request processing failed; nested exception is org.springframework.jdbc.BadSqlGrammarException: 
### Error querying database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'bright.country' doesn't exist
### The error may exist in com/wosai/bright/mapper/CountryMapper.java (best guess)
### The error may involve com.wosai.bright.mapper.CountryMapper.selectByExample-Inline
### The error occurred while setting parameters
### SQL: SELECT count(0) FROM country
### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'bright.country' doesn't exist
; bad SQL grammar []; nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: Table 'bright.country' doesn't exist
```
- 解决办法: 检查`config.properties`中` jdbc.url `内容是否正确，特别是`ip`
```properties
## MySqlConfig
jdbc.type=mysql
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/bright
jdbc.username=root
jdbc.password=
```


---
# `Column 'id' cannot be null` 列`id`不能为空
``` text
2017-03-10 15:36:15,334 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Could not complete request
org.springframework.dao.DataIntegrityViolationException: 
### Error updating database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Column 'id' cannot be null
### The error may involve com.wosai.bright.mapper.CountryMapper.insert-Inline
### The error occurred while setting parameters
### SQL: INSERT INTO country  ( Id,countryname,countrycode ) VALUES( ?,?,? )
### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Column 'id' cannot be null
; SQL []; Column 'id' cannot be null; nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Column 'id' cannot be null
```
- 解决办法:  查看数据库是否中id是否`自动递增`
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/21388886.jpg)

 
其次方向:`检查实体类`
[`com.mysql.jdbc.exceptions.jdbc4.MySQLIntegrityConstraintViolationException: Column 'id' cannot be null，无法自动生成主键`]( http://git.oschina.net/baomidou/mybatis-plus/issues/174)


---
# 分页控制
```xml
org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.binding.BindingException: Parameter 'countryname' not found. Available parameters are [0, pageSize, param3, pageNum, param1, param2]

```
- 起因
```
参考:  调用方式,第四种 
https://github.com/pagehelper/Mybatis-PageHelper/blob/master/wikis/zh/HowToUse.md 
```
- 解决办法:
`PageHelper.startPage(page, rows);// 辅助分页`
```java
@Override
public List<Country> selectByCountry_sql(Country country, final int page, final int rows) {
    Map<String, Object> map = new HashedMap();
    map.put("countryname", country.getCountryname());
    map.put("countrycode", country.getCountrycode());


    PageHelper.startPage(page, rows);//  辅助 分页


    List<Country> countryList;
    countryList = countryMapper.selectByCountryQueryModel(map);// ok
//        countryList = countryMapper.selectByCountryQueryModel(country);// ok
//        countryList = countryMapper.selectByCountryQueryModel(map, page, rows);// error
//        countryList = countryMapper.selectByCountryQueryModel(country, page, rows);// error


    return countryList;
}
```


---
# `shiro`  @RequiresPermissions无法触发
不会调用授权验证方法 ` userRealm.java -  doGetAuthorizationInfo 授权(验证权限时调用)`
```java
@RequestMapping("testAnnotation")
@RequiresPermissions("sys:schedule:log")
public void testAnnotation(HttpServletRequest request, HttpServletResponse response) {
    ...

}
```
- 解决办法：
在`spring-servlet.xml`中添加`shiro`拦截器支持.
```xml
<!-- 支持Shiro对Controller的方法级AOP安全控制 begin-->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
 depends-on="lifecycleBeanPostProcessor">
<property name="proxyTargetClass" value="true" />
</bean>
```
注意: `spring-servlet.xml`中添加的 `lifecycleBeanPostProcessor`在IDE中会显示红色(因无法检测到引用). 
实则只要确保`spring-context-shiro.xml`中声明了`<bean id="lifecycleBeanPostProcessor"`就能正常加载.
```xml
<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>


<!-- AOP式方法级权限检查  -->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
      depends-on="lifecycleBeanPostProcessor">
    <property name="proxyTargetClass" value="true"/>
</bean>
<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
    <property name="securityManager" ref="securityManager"/>
</bean>
```
![]( http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/19801636.jpg)


 - 标红问题，可以给项目加入`Spring`支持，就能关联到相关参数
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/78509387.jpg)



---
#   java.sql.SQLException: 无效的列类型: 1111
```text
严重: Servlet.service() for servlet [springServlet] in context with path [] threw exception [Request processing failed; nested exception is org.springframework.jdbc.UncategorizedSQLException: Error setting null for parameter #1 with JdbcType OTHER . Try setting a different JdbcType for this parameter or a different jdbcTypeForNull configuration property. Cause: java.sql.SQLException: 无效的列类型: 1111
; uncategorized SQLException for SQL []; SQL state [99999]; error code [17004]; 无效的列类型: 1111; nested exception is java.sql.SQLException: 无效的列类型: 1111] with root cause
java.sql.SQLException: 无效的列类型: 1111
at oracle.jdbc.driver.OracleStatement.getInternalType(OracleStatement.java:4188)
at oracle.jdbc.driver.OraclePreparedStatement.setNullCritical(OraclePreparedStatement.java:4858)
at oracle.jdbc.driver.OraclePreparedStatement.setNull(OraclePreparedStatement.java:4840)
at oracle.jdbc.driver.OraclePreparedStatementWrapper.setNull(OraclePreparedStatementWrapper.java:1292)
```
- 问题原因：插入控制，缺少数据类型
- 解决办法(治标-> ORA-01400: 无法将 NULL 插入 )：
`mybatis-config.xml`

```xml
<settings>

<!-- 设置但JDBC类型为空时,某些驱动程序 要指定值,default:OTHER，插入空值时不需要指定类型 -->
<setting name="jdbcTypeForNull" value="NULL"/>
</settings>
```
**参考 **
`java+oracle数据库调用updateByPrimaryKeySelective、insertSelective等方法抛异常，无效的列类型: 1111`
https://github.com/abel533/Mapper/issues/29



`mybatis报错：java.sql.SQLException: 无效的列类型: 1111 - 美丽的泡沫 - 博客频道 - CSDN.NET`
http://blog.csdn.net/stronglyh/article/details/45369611


`MYBATIS 无效的列类型: 1111 - 菠萝啊哈哈`
https://my.oschina.net/u/232879/blog/210086


`Oracle数据库mybatis 插入空值时报错（with JdbcType OTHER） - fishernemo的专栏 CSDN.NET`
http://blog.csdn.net/fishernemo/article/details/27649233


- 解决办法(治本 )：

```java
/**
 * 主键_oracle
 */
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY, generator = "select seq_country.nextval from dual")
private Integer id;
```


```xml
- mybatis保证oracle序列先执行`ORDER=BEFORE`



<!-- @MyBatisDao注解的接口(Mapper<Entity>类支持) -->
<bean id="mapperScannerConfigurer" class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.wosai.bright.mapper"/>
    <!-- 3.2.2版本新特性，markerInterface可以起到mappers配置的作用，详细情况需要看Marker接口类 -->
    <property name="markerInterface" value="com.wosai.bright.common.MyMapper"/>
    <!-- 通用Mapper通过属性注入进行配置，默认不配置时会注册Mapper<T>接口-->
    <property name="properties">
        <value>
            mappers=tk.mybatis.mapper.common.Mapper
            ORDER=BEFORE
        </value>
    </property>
</bean>
```


---
# ORA-01400: 无法将 NULL 插入 ("C##WOSAI"."COUNTRY"."ID")
```text
org.springframework.web.util.NestedServletException: Request processing failed; nested exception is org.springframework.dao.DataIntegrityViolationException: 
### Error updating database.  Cause: java.sql.SQLIntegrityConstraintViolationException: ORA-01400: 无法将 NULL 插入 ("C##WOSAI"."COUNTRY"."ID")


### The error may involve com.wosai.bright.mapper.CountryMapper.insert-Inline
### The error occurred while setting parameters
### SQL: INSERT INTO country  ( id,countryname,countrycode ) VALUES( ?,?,? )
### Cause: java.sql.SQLIntegrityConstraintViolationException: ORA-01400: 无法将 NULL 插入 ("C##WOSAI"."COUNTRY"."ID")


; SQL []; ORA-01400: 无法将 NULL 插入 ("C##WOSAI"."COUNTRY"."ID")
; nested exception is java.sql.SQLIntegrityConstraintViolationException: ORA-01400: 无法将 NULL 插入 ("C##WOSAI"."COUNTRY"."ID")
```
问题原因： 插入空id，是因为序列值不能正常生产
- 解决办法
```java
/**
 * 主键_oracle
 */
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY, generator = "select seq_country.nextval from dual")
private Integer id;
```


```xml
- mybatis保证oracle序列先执行`ORDER=BEFORE`



<!-- @MyBatisDao注解的接口(Mapper<Entity>类支持) -->
<bean id="mapperScannerConfigurer" class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="com.wosai.bright.mapper"/>
    <!-- 3.2.2版本新特性，markerInterface可以起到mappers配置的作用，详细情况需要看Marker接口类 -->
    <property name="markerInterface" value="com.wosai.bright.common.MyMapper"/>
    <!-- 通用Mapper通过属性注入进行配置，默认不配置时会注册Mapper<T>接口-->
    <property name="properties">
        <value>
            mappers=tk.mybatis.mapper.common.Mapper
            ORDER=BEFORE
        </value>
    </property>
</bean>
```


`oracle错误(二) ORA-01400: 无法将 NULL 插入 ("SL"."TEMP_TEST_TABLE"."ID")的解决方案`
http://blog.csdn.net/zengdeqing2012/article/details/46376493


---
# 启动异常
` No qualifying bean of type [tk.mybatis.mapper.common.Mapper] found for dependency `
没有找到类型为[tk.mybatis.mapper.common.Mapper]的合格的bean



完整日志：
```text
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sysConfigController': Injection of autowired dependencies failed; nested exception is org.springframework.beans.factory.BeanCreationException: Could not autowire field: private com.wosai.bright.service.SysConfigService com.wosai.bright.controller_web.system.SysConfigController.sysConfigService; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sysConfigService': Injection of autowired dependencies failed; nested exception is org.springframework.beans.factory.BeanCreationException: Could not autowire field: protected tk.mybatis.mapper.common.Mapper com.wosai.bright.service.impl.BaseService.mapper; nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [tk.mybatis.mapper.common.Mapper] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}

```
- 解决办法：
更新`config.properties`文件
```properties
mapper.Mapper = tk.mybatis.mapper.common.Mapper

修改为
mapper.Mapper = com.wosai.bright.common.MyMapper
```
- 解决方法2：
`spring-context.xml`，更新` markerInterface `值为` tk.mybatis.mapper.common.Mapper`
或注释`  <property name="markerInterface"   `
```xml
<!-- @MyBatisDao注解的接口(Mapper<Entity>类支持) -->
<bean id="mapperScannerConfigurer" class="tk.mybatis.spring.mapper.MapperScannerConfigurer">
   <property name="markerInterface" value="tk.mybatis.mapper.common.Mapper"/>

```
**参考**
`config.properties 修改`
https://github.com/abel533/Mybatis-Spring/issues/16


`mybatis + spring 4 集成，使用通用mapper报错 - 开源中国社区`
http://www.oschina.net/question/852516_2141712?sort=time
http://www.codes51.com/itwd/2435755.html


---
# 启动卡死` PluggableSchemaResolver] - Loaded schema mappings `
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/1533464.jpg)
```text
2017-03-18 20:56:55,287 DEBUG [springframework.beans.factory.xml.PluggableSchemaResolver] - Loaded schema mappings: {http://www.springframework.org/schema/tx/spring-tx-2.5.xsd=org/springframework/transaction/config/spring-tx-2.5.xsd, http://www.springframework.org/schema/aop/spring-aop-4.1.xsd=org/springframework/aop/config/spring-aop-4.1.xsd, http://www.springframework.org/schema/context/spring-context-3.1.xsd=org/springframework/context/config/spring-context-3.1.xsd, http://www.springframework.org/schema/jdbc/spring-jdbc-4.1.xsd=org/springframework/jdbc/config/spring-jdbc-4.1.xsd, http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd=org/springframework/web/servlet/config/spring-mvc-4.1.xsd, http://www.springframework.org/schema/util/spring-util-3.0.xsd=org/springframework/beans/factory/xml/spring-util-3.0.xsd, http://www.springframework.org/schema/security/spring-security-3.0.3.xsd=org/springframework/security/config/spring-security-3.0.3.xsd, http://www.springframework.org/schema/security/spring-security-2.0.1.xsd=org/springframework/security/config/spring-security-2.0.1.xsd, http://www.springframework.org/schema/tool/spring-tool.xsd=org/springframework/beans/factory/xml/spring-tool-4.1.xsd, http://www.springframework.org/schema/aop/spring-aop-3.2.xsd=org/springframework/aop/config/spring-aop-3.2.xsd, http://mybatis.org/schema/mybatis-spring-1.2.xsd=org/mybatis/spring/config/mybatis-spring-1.2.xsd, http://www.springframework.org/schema/lang/spring-lang-4.1.xsd=org/springframework/scripting/config/spring-lang-4.1.xsd, http://www.springframework.org/schema/context/spring-context-4.0.xsd=org/springframework/context/config/spring-context-4.0.xsd, http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd=org/springframework/web/servlet/config/spring-mvc-3.2.xsd, http://www.springframework.org/schema/oxm/spring-oxm-3.0.xsd=org/springframework/oxm/config/spring-oxm-3.0.xsd, http://www.springframework.org/schema/tool/spring-tool-4.1.xsd=org/springframework/beans/factory/xml/spring-tool-4.1.xsd, http://www.springframework.org/schema/lang/spring-lang-3.2.xsd=org/springframework/scripting/config/spring-lang-3.2.xsd, http://www.springframework.org/schema/cache/spring-cache-3.2.xsd=org/springframework/cache/config/spring-cache-3.2.xsd, http://www.springframework.org/schema/jee/spring-jee-4.1.xsd=org/springframework/ejb/config/spring-jee-4.1.xsd, http://www.springframework.org/schema/jdbc/spring-jdbc-3.1.xsd=org/springframework/jdbc/config/spring-jdbc-3.1.xsd, http://www.springframework.org/schema/util/spring-util-2.0.xsd=org/springframework/beans/factory/xml/spring-util-2.0.xsd, http://www.springframework.org/schema/tool/spring-tool-3.2.xsd=org/springframework/beans/factory/xml/spring-tool-3.2.xsd, http://www.springframework.org/schema/context/spring-context.xsd=org/springframework/context/config/spring-context-4.1.xsd, http://www.springframework.org/schema/cache/spring-cache-4.1.xsd=org/springframework/cache/config/spring-cache-4.1.xsd, http://www.springframework.org/schema/aop/spring-aop-4.0.xsd=org/springframework/aop/config/spring-aop-4.0.xsd, http://www.springframework.org/schema/security/spring-security-2.0.xsd=org/springframework/security/config/spring-security-2.0.xsd, http://www.springframework.org/schema/jee/spring-jee-3.2.xsd=org/springframework/ejb/config/spring-jee-3.2.xsd, http://www.springframework.org/schema/context/spring-context-3.0.xsd=org/springframework/context/config/spring-context-3.0.xsd, http://www.springframework.org/schema/jdbc/spring-jdbc-4.0.xsd=org/springframework/jdbc/config/spring-jdbc-4.0.xsd, http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd=org/springframework/web/servlet/config/spring-mvc-4.0.xsd, http://www.springframework.org/schema/util/spring-util-2.5.xsd=org/springframework/beans/factory/xml/spring-util-2.5.xsd, http://www.springframework.org/schema/beans/spring-beans-3.2.xsd=org/springframework/beans/factory/xml/spring-beans-3.2.xsd, http://www.springframework.org/schema/aop/spring-aop-3.1.xsd=org/springframework/aop/config/spring-aop-3.1.xsd, http://www.springframework.org/schema/lang/spring-lang-4.0.xsd=org/springframework/scripting/config/spring-lang-4.0.xsd, http://www.springframework.org/schema/mvc/spring-mvc.xsd=org/springframework/web/servlet/config/spring-mvc-4.1.xsd, http://www.springframework.org/schema/mvc/spring-mvc-3.1.xsd=org/springframework/web/servlet/config/spring-mvc-3.1.xsd, http://www.springframework.org/schema/beans/spring-beans-4.1.xsd=org/springframework/beans/factory/xml/spring-beans-4.1.xsd, http://www.springframework.org/schema/tool/spring-tool-4.0.xsd=org/springframework/beans/factory/xml/spring-tool-4.0.xsd, http://www.springframework.org/schema/tx/spring-tx-3.2.xsd=org/springframework/transaction/config/spring-tx-3.2.xsd, http://www.springframework.org/schema/lang/spring-lang-3.1.xsd=org/springframework/scripting/config/spring-lang-3.1.xsd, http://www.springframework.org/schema/cache/spring-cache-3.1.xsd=org/springframework/cache/config/spring-cache-3.1.xsd, http://www.springframework.org/schema/jee/spring-jee-4.0.xsd=org/springframework/ejb/config/spring-jee-4.0.xsd, http://www.springframework.org/schema/jdbc/spring-jdbc-3.0.xsd=org/springframework/jdbc/config/spring-jdbc-3.0.xsd, http://www.springframework.org/schema/task/spring-task-4.1.xsd=org/springframework/scheduling/config/spring-task-4.1.xsd, http://www.springframework.org/schema/jdbc/spring-jdbc.xsd=org/springframework/jdbc/config/spring-jdbc-4.1.xsd, http://www.springframework.org/schema/tool/spring-tool-3.1.xsd=org/springframework/beans/factory/xml/spring-tool-3.1.xsd, http://www.springframework.org/schema/tx/spring-tx-4.1.xsd=org/springframework/transaction/config/spring-tx-4.1.xsd, http://www.springframework.org/schema/cache/spring-cache-4.0.xsd=org/springframework/cache/config/spring-cache-4.0.xsd, http://www.springframework.org/schema/jee/spring-jee-3.1.xsd=org/springframework/ejb/config/spring-jee-3.1.xsd, http://www.springframework.org/schema/task/spring-task-3.2.xsd=org/springframework/scheduling/config/spring-task-3.2.xsd, http://www.springframework.org/schema/beans/spring-beans-3.1.xsd=org/springframework/beans/factory/xml/spring-beans-3.1.xsd, http://www.springframework.org/schema/util/spring-util.xsd=org/springframework/beans/factory/xml/spring-util-4.1.xsd, http://www.springframework.org/schema/aop/spring-aop-3.0.xsd=org/springframework/aop/config/spring-aop-3.0.xsd, http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd=org/springframework/web/servlet/config/spring-mvc-3.0.xsd, http://www.springframework.org/schema/beans/spring-beans-4.0.xsd=org/springframework/beans/factory/xml/spring-beans-4.0.xsd, http://www.springframework.org/schema/beans/spring-beans.xsd=org/springframework/beans/factory/xml/spring-beans-4.1.xsd, http://www.springframework.org/schema/security/spring-security-2.0.4.xsd=org/springframework/security/config/spring-security-2.0.4.xsd, http://www.springframework.org/schema/tx/spring-tx-3.1.xsd=org/springframework/transaction/config/spring-tx-3.1.xsd, http://www.springframework.org/schema/lang/spring-lang-3.0.xsd=org/springframework/scripting/config/spring-lang-3.0.xsd, http://mybatis.org/schema/mybatis-spring.xsd=org/mybatis/spring/config/mybatis-spring-1.2.xsd, http://www.springframework.org/schema/context/spring-context-2.5.xsd=org/springframework/context/config/spring-context-2.5.xsd, http://www.springframework.org/schema/task/spring-task-4.0.xsd=org/springframework/scheduling/config/spring-task-4.0.xsd, http://www.springframework.org/schema/tool/spring-tool-3.0.xsd=org/springframework/beans/factory/xml/spring-tool-3.0.xsd, http://www.springframework.org/schema/tx/spring-tx-4.0.xsd=org/springframework/transaction/config/spring-tx-4.0.xsd, http://www.springframework.org/schema/aop/spring-aop-2.0.xsd=org/springframework/aop/config/spring-aop-2.0.xsd, http://www.springframework.org/schema/security/spring-security-3.2.xsd=org/springframework/security/config/spring-security-3.2.xsd, http://www.springframework.org/schema/jee/spring-jee-3.0.xsd=org/springframework/ejb/config/spring-jee-3.0.xsd, http://www.springframework.org/schema/util/spring-util-4.1.xsd=org/springframework/beans/factory/xml/spring-util-4.1.xsd, http://www.springframework.org/schema/task/spring-task-3.1.xsd=org/springframework/scheduling/config/spring-task-3.1.xsd, http://www.springframework.org/schema/beans/spring-beans-3.0.xsd=org/springframework/beans/factory/xml/spring-beans-3.0.xsd, http://www.springframework.org/schema/jee/spring-jee.xsd=org/springframework/ejb/config/spring-jee-4.1.xsd, http://www.springframework.org/schema/aop/spring-aop-2.5.xsd=org/springframework/aop/config/spring-aop-2.5.xsd, http://www.springframework.org/schema/lang/spring-lang-2.0.xsd=org/springframework/scripting/config/spring-lang-2.0.xsd, http://www.springframework.org/schema/oxm/spring-oxm.xsd=org/springframework/oxm/config/spring-oxm-4.1.xsd, http://www.springframework.org/schema/util/spring-util-3.2.xsd=org/springframework/beans/factory/xml/spring-util-3.2.xsd, http://www.springframework.org/schema/oxm/spring-oxm-4.1.xsd=org/springframework/oxm/config/spring-oxm-4.1.xsd, http://www.springframework.org/schema/task/spring-task.xsd=org/springframework/scheduling/config/spring-task-4.1.xsd, http://www.springframework.org/schema/tool/spring-tool-2.0.xsd=org/springframework/beans/factory/xml/spring-tool-2.0.xsd, http://www.springframework.org/schema/tx/spring-tx-3.0.xsd=org/springframework/transaction/config/spring-tx-3.0.xsd, http://www.springframework.org/schema/lang/spring-lang-2.5.xsd=org/springframework/scripting/config/spring-lang-2.5.xsd, http://www.springframework.org/schema/jee/spring-jee-2.0.xsd=org/springframework/ejb/config/spring-jee-2.0.xsd, http://www.springframework.org/schema/oxm/spring-oxm-3.2.xsd=org/springframework/oxm/config/spring-oxm-3.2.xsd, http://www.springframework.org/schema/tool/spring-tool-2.5.xsd=org/springframework/beans/factory/xml/spring-tool-2.5.xsd, http://www.springframework.org/schema/security/spring-security-3.1.xsd=org/springframework/security/config/spring-security-3.1.xsd, http://www.springframework.org/schema/jee/spring-jee-2.5.xsd=org/springframework/ejb/config/spring-jee-2.5.xsd, http://www.springframework.org/schema/util/spring-util-4.0.xsd=org/springframework/beans/factory/xml/spring-util-4.0.xsd, http://www.springframework.org/schema/task/spring-task-3.0.xsd=org/springframework/scheduling/config/spring-task-3.0.xsd, http://www.springframework.org/schema/lang/spring-lang.xsd=org/springframework/scripting/config/spring-lang-4.1.xsd, http://www.springframework.org/schema/context/spring-context-3.2.xsd=org/springframework/context/config/spring-context-3.2.xsd, http://www.springframework.org/schema/util/spring-util-3.1.xsd=org/springframework/beans/factory/xml/spring-util-3.1.xsd, http://www.springframework.org/schema/beans/spring-beans-2.0.xsd=org/springframework/beans/factory/xml/spring-beans-2.0.xsd, http://www.springframework.org/schema/oxm/spring-oxm-4.0.xsd=org/springframework/oxm/config/spring-oxm-4.0.xsd, http://www.springframework.org/schema/cache/spring-cache.xsd=org/springframework/cache/config/spring-cache-4.1.xsd, http://www.springframework.org/schema/tx/spring-tx.xsd=org/springframework/transaction/config/spring-tx-4.1.xsd, http://www.springframework.org/schema/security/spring-security-2.0.2.xsd=org/springframework/security/config/spring-security-2.0.2.xsd, http://www.springframework.org/schema/context/spring-context-4.1.xsd=org/springframework/context/config/spring-context-4.1.xsd, http://www.springframework.org/schema/beans/spring-beans-2.5.xsd=org/springframework/beans/factory/xml/spring-beans-2.5.xsd, http://www.springframework.org/schema/oxm/spring-oxm-3.1.xsd=org/springframework/oxm/config/spring-oxm-3.1.xsd, http://www.springframework.org/schema/tx/spring-tx-2.0.xsd=org/springframework/transaction/config/spring-tx-2.0.xsd, http://www.springframework.org/schema/security/spring-security.xsd=org/springframework/security/config/spring-security-3.2.xsd, http://www.alibaba.com/schema/stat.xsd=META-INF/stat.xsd, http://www.springframework.org/schema/security/spring-security-3.0.xsd=org/springframework/security/config/spring-security-3.0.xsd, http://www.springframework.org/schema/jdbc/spring-jdbc-3.2.xsd=org/springframework/jdbc/config/spring-jdbc-3.2.xsd, http://www.springframework.org/schema/aop/spring-aop.xsd=org/springframework/aop/config/spring-aop-4.1.xsd}
```
- 猜测网络资源无法加载，尝试断网启动再分析
` http://www.springframework.org/... `


# 断网启动错误：` the root element of the document is not <xsd:schema> `
```text
org.xml.sax.SAXParseException; lineNumber: 17; columnNumber: 70; schema_reference.4: Failed to read schema document 'http://www.springframework.org/schema/beans/spring-beans-4.2.xsd', because 1) could not find the document; 2) the document could not be read; 3) the root element of the document is not <xsd:schema>.

```
```text
org.springframework.beans.factory.xml.XmlBeanDefinitionStoreException: Line 19 in XML document from file [G:\work_code\intelligentBuilding\bright\target\bright\WEB-INF\classes\spring\spring-context.xml] is invalid; nested exception is org.xml.sax.SAXParseException; lineNumber: 19; columnNumber: 33; cvc-elt.1: Cannot find the declaration of element 'beans'.

```
- 解决办法：
各spring配置文件，引入的`xsd`文件版本号最好是一致的。绝不可配置更高的版本，否则就会出现此问题，找不到 目标 文件。


-  工程中依赖到不同的版本的spring,会造成此问题
```text
在实现开发中，出现上述错误的几率并不高，最常见的导致这一问题的原因其实与使用了一个名为“assembly”的maven打包插件有关。很多项目需要将工程连同其所依赖的所有jar包打包成一个jar包，maven的assembly插件就是用来完成这个任务的。但是由于工程往往依赖很多的jar包，而被依赖的jar又会依赖其他的jar包，这样，当工程中依赖到不同的版本的spring时，在使用assembly进行打包时，只能将某一个版本jar包下的spring.schemas文件放入最终打出的jar包里，这就有可能遗漏了一些版本的xsd的本地映射，进而出现了文章开始提到的错误。如果你的项目是打成单一jar的，你可以通过检查最终生成的jar里的spring.schemas文件来确认是不是这种情况。而关于这种情况，解决的方法一般是推荐使用另外一种打包插件shade,它确实是一款比assembly更加优秀的工具，在对spring.schemas文件处理上，shade能够将所有jar里的spring.schemas文件进行合并，在最终生成的单一jar包里，spring.schemas包含了所有出现过的版本的集合！

```
`Spring如何加载XSD文件(org.xml.sax.SAXParseException: Failed to read schema document错误的解决方法)`
http://blog.csdn.net/bluishglc/article/details/7596118


- 建议使用无版本的`xsd`, 因为它们映射到您在应用程序中使用的当前版本的框架
```text
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
而不是
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd

```
```text
建议使用“无版本”XSD，因为它们映射到您在应用程序中使用的当前版本的框架。


应用程序和工具不应该尝试从Web获取这些XSD，因为这些模式包含在JAR中。如果他们这样做，通常意味着您的应用程序试图使用比您使用的框架版本更新的XSD，或者您的IDE /工具未正确配置。


据我所知，只有一种情况，你想使用特定的XSD版本：当尝试使用一个已被弃用/修改在一个更新的版本的XML属性。这不会经常发生，至少说
```
`Spring配置XML模式：有没有版本？ `
http://stackoverflow.com/questions/20894695/spring-configuration-xml-schema-with-or-without-version/20900801#20900801



技巧：按着Ctrl键，鼠标选择会出现关联本地的文件

![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/46243355.jpg)



相关配置：（统一或者无版本配置[默认当前jar包版本]）
![](http://7xnbs3.com1.z0.glb.clouddn.com/17-8-12/88780676.jpg)







**参考**
`Spring如何加载XSD文件(org.xml.sax.SAXParseException: Failed to read schema document错误的解决方法)`
http://blog.csdn.net/bluishglc/article/details/7596118



`关于xml配置文件无元素提示和the root element of the document is not <xsd:schema>.错误 CSDN.NET`

http://blog.csdn.net/qq_27832819/article/details/52126388


`spring - SAXParseException, the root element of the document is not <xsd:schema> - Stack Overflow`
http://stackoverflow.com/questions/28902500/saxparseexception-the-root-element-of-the-document-is-not-xsdschema



`java - Spring configuration XML schema: with or without version? - Stack Overflow`
http://stackoverflow.com/questions/20894695/spring-configuration-xml-schema-with-or-without-version/20900801#20900801


---
# expected single matching bean but found 2: countryService,sysConfigService
将单匹配豆却发现2：countryservice，sysconfigservice

```text
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sysConfigController': Injection of autowired dependencies failed; nested exception is org.springframework.beans.factory.BeanCreationException: Could not autowire field: private com.wosai.bright.service.IService com.wosai.bright.controller_web.system.SysConfigController.iService; nested exception is org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type [com.wosai.bright.service.IService] is defined: expected single matching bean but found 2: countryService,sysConfigService

```
- 解决办法：不能为多实现的接口类进行spring自动注入` @Autowired `
```java
/**
 * 不建议,spring的默认注入会使用已经实现IService接口的实例类(如:CountryServiceImpl)
 * 或者发现有多个实现,将会报错:expected single matching bean but found 2: countryService,sysConfigService
 */
@SuppressWarnings("SpringJavaAutowiringInspection")
@Autowired
private IService iService;
```


---
# 请求后，响应了xml
```
<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<report-controller$1 empty=\"false\"/>

```
- 请求报文分析：

返回json：
```
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

```
返回xml：
```
Accept:*/*

```
- 服务端日志分析
返回json：

```
2017-03-20 13:51:38,596 DEBUG [org.springframework.web.servlet.DispatcherServlet] - DispatcherServlet with name 'springServlet' processing GET request for [/API/monitor/report]
2017-03-20 13:51:38,596 DEBUG [servlet.mvc.method.annotation.RequestMappingHandlerMapping] - Looking up handler method for path /API/monitor/report
2017-03-20 13:51:38,597 DEBUG [servlet.mvc.method.annotation.RequestMappingHandlerMapping] - Returning handler method [public java.util.Map<java.lang.String, java.lang.Object> com.wosai.bright.controller_api.monitor.ReportController.report(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)]
2017-03-20 13:51:38,597 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Returning cached instance of singleton bean 'reportController'
2017-03-20 13:51:38,597 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Last-Modified value for [/API/monitor/report] is: -1
2017-03-20 13:51:38,599 DEBUG [servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor] - Written [{message=GET pram:demo2 is received}] as "text/html" using [org.springframework.http.converter.json.MappingJackson2HttpMessageConverter@3534de84]
2017-03-20 13:51:38,599 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Null ModelAndView returned to DispatcherServlet with name 'springServlet': assuming HandlerAdapter completed request handling
2017-03-20 13:51:38,599 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Successfully completed request
```
返回xml：

```
2017-03-20 13:52:03,736 DEBUG [org.springframework.web.servlet.DispatcherServlet] - DispatcherServlet with name 'springServlet' processing GET request for [/API/monitor/report]
2017-03-20 13:52:03,736 DEBUG [servlet.mvc.method.annotation.RequestMappingHandlerMapping] - Looking up handler method for path /API/monitor/report
2017-03-20 13:52:03,737 DEBUG [servlet.mvc.method.annotation.RequestMappingHandlerMapping] - Returning handler method [public java.util.Map<java.lang.String, java.lang.Object> com.wosai.bright.controller_api.monitor.ReportController.report(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)]
2017-03-20 13:52:03,737 DEBUG [springframework.beans.factory.support.DefaultListableBeanFactory] - Returning cached instance of singleton bean 'reportController'
2017-03-20 13:52:03,737 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Last-Modified value for [/API/monitor/report] is: -1


2017-03-20 13:52:03,738 DEBUG [org.exolab.castor.xml.XMLContext] - Creating new Marshaller instance.
2017-03-20 13:52:03,738 DEBUG [org.exolab.castor.xml.Marshaller] - Marshalling com.wosai.bright.controller_api.monitor.ReportController$1
2017-03-20 13:52:03,738 DEBUG [exolab.castor.xml.util.XMLClassDescriptorResolverImpl$DescriptorCacheImpl] - Get descriptor for: com.wosai.bright.controller_api.monitor.ReportController$1 found: org.exolab.castor.xml.IntrospectedXMLClassDescriptor@2b4477f6; descriptor for class: com.wosai.bright.controller_api.monitor.ReportController$1; xml name: null
2017-03-20 13:52:03,738 DEBUG [exolab.castor.xml.util.XMLClassDescriptorResolverImpl$DescriptorCacheImpl] - Get descriptor for: com.wosai.bright.controller_api.monitor.ReportController$1 found: org.exolab.castor.xml.IntrospectedXMLClassDescriptor@2b4477f6; descriptor for class: com.wosai.bright.controller_api.monitor.ReportController$1; xml name: null


2017-03-20 13:52:03,739 DEBUG [servlet.mvc.method.annotation.RequestResponseBodyMethodProcessor] - Written [{message=GET pram:demo2 is received}] as "application/xml" using [org.springframework.http.converter.xml.MarshallingHttpMessageConverter@2bccabf2]
2017-03-20 13:52:03,739 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Null ModelAndView returned to DispatcherServlet with name 'springServlet': assuming HandlerAdapter completed request handling
2017-03-20 13:52:03,739 DEBUG [org.springframework.web.servlet.DispatcherServlet] - Successfully completed request
```

分析,返回xml的请求，多出了` org.exolab.castor.xml.Marshaller `的处理
```
2017-03-20 13:52:03,738 DEBUG [org.exolab.castor.xml.XMLContext] - Creating new Marshaller instance.
2017-03-20 13:52:03,738 DEBUG [org.exolab.castor.xml.Marshaller] - Marshalling com.wosai.bright.controller_api.monitor.ReportController$1
2017-03-20 13:52:03,738 DEBUG [exolab.castor.xml.util.XMLClassDescriptorResolverImpl$DescriptorCacheImpl] - Get descriptor for: com.wosai.bright.controller_api.monitor.ReportController$1 found: org.exolab.castor.xml.IntrospectedXMLClassDescriptor@2b4477f6; descriptor for class: com.wosai.bright.controller_api.monitor.ReportController$1; xml name: null
2017-03-20 13:52:03,738 DEBUG [exolab.castor.xml.util.XMLClassDescriptorResolverImpl$DescriptorCacheImpl] - Get descriptor for: com.wosai.bright.controller_api.monitor.ReportController$1 found: org.exolab.castor.xml.IntrospectedXMLClassDescriptor@2b4477f6; descriptor for class: com.wosai.bright.controller_api.monitor.ReportController$1; xml name: null
```


- 解决办法：
调整消息转换器的顺序
```
<mvc:annotation-driven>
<!-- 消息转换(常用于:返回字符串,对象转json) -->
<mvc:message-converters>
<ref bean="stringHttpMessageConverter"/>
<ref bean="marshallingHttpMessageConverter"/>

<ref bean="mappingJackson2HttpMessageConverter"/>
</mvc:message-converters>
</mvc:annotation-driven>
```
修改为(json 转换器 `mappingJackson2HttpMessageConverter`提到xml转换器之前 )
```
<mvc:annotation-driven>
<!-- 消息转换(常用于:返回字符串,对象转json) -->
<mvc:message-converters>
<ref bean="stringHttpMessageConverter"/>
<ref bean="mappingJackson2HttpMessageConverter"/>
<ref bean="marshallingHttpMessageConverter"/>
</mvc:message-converters>
</mvc:annotation-driven>
```
