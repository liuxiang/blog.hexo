title: 【java脚手架搭建】调度 spring-servlet.xml
date: 2017-3-8 00:00:03
tags: [ 脚手架 ]


---
# `spring-servlet.xml` 或`spring-mvc.xml`相关配置


## MVC注解支持 & 数据处理
```xml
<!--RequestMappingHandlerAdapter-->
<!-- 默认的注解映射的支持，org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping -->
<mvc:annotation-driven>
    <!-- 消息转换(常用于:返回字符串,对象转json) -->

  <mvc:message-converters>
    <ref bean="stringHttpMessageConverter"/>
    <ref bean="marshallingHttpMessageConverter"/>
    <ref bean="mappingJackson2HttpMessageConverter"/>
  </mvc:message-converters>
</mvc:annotation-driven>


<!-- `@RequestMapping`注解生效(servlet映射) -->
<context:component-scan base-package="com.wosai.bright.controller"/>


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


## REST中根据URL后缀自动判定Content-Type及相应的View
```xml
<!-- REST中根据URL后缀自动判定Content-Type及相应的View -->
<bean id="contentNegotiationManager" class="org.springframework.web.accept.ContentNegotiationManagerFactoryBean">
<property name="mediaTypes" >
<map> 
<entry key="xml" value="application/xml"/> 
<entry key="json" value="application/json"/> 
</map>
</property>
<property name="ignoreAcceptHeader" value="true"/>
<property name="favorPathExtension" value="true"/>
</bean>
```


---
## 定义视图文件解析
```xml
<!-- 定义视图文件解析 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="${web.view.prefix}"/>
<property name="suffix" value="${web.view.suffix}"/>
</bean>
```
```properties
web.view.prefix=/WEB-INF/views/
web.view.suffix=.jsp
```
或
```xml
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/views/" />
<!-- 取消自动后缀,程序主动填写能同时使用.jsp和.html的文件 -->
<!--<property name="suffix" value=".jsp" />-->
</bean>
```


---
## 文件上传
```xml
<!--文件上传-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  <property name="maxUploadSize" value="100000"/>
</bean>
```
或
```xml
<!-- 上传文件拦截，设置最大上传文件大小   10M=10*1024*1024(B)=10485760 bytes -->  
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">  
<property name="maxUploadSize" value="${web.maxUploadSize}" />  
</bean>
```
## 国际化
```xml
<!--国际化-->
<bean id="messageSource"
      class="org.springframework.context.support.ResourceBundleMessageSource">
  <property name="defaultEncoding" value="UTF-8"/>
  <property name="basenames">
    <list>
      <value>messages.welcome</value>
    </list>
  </property>
</bean>
```


## 静态资源(不经过servlet,filter)
```xml
<!-- 对静态资源文件的访问， 将无法mapping到Controller的path交给default servlet handler处理 -->
<mvc:default-servlet-handler />


<!-- 静态资源映射 -->
<mvc:resources mapping="/static/**" location="/static/" cache-period="31536000"/>


<!-- 定义无Controller的path<->view直接映射 -->
<mvc:view-controller path="/" view-name="redirect:${web.view.index}"/> <!-- redirect:/index -->
```
```xml
<!-- 静态资源 -->
<mvc:resources location="/oss/js/" mapping="/oss/js/**" />
<mvc:resources location="/oss/temp/" mapping="/oss/temp/**" />
<mvc:resources location="/oss/images/" mapping="/oss/images/**" />
<mvc:resources location="/oss/B-JUI/" mapping="/oss/B-JUI/**" />
<mvc:resources location="/oss/lib/" mapping="/oss/lib/**" />
<mvc:resources location="/oss/emoji/" mapping="/oss/emoji/**" />
<mvc:resources location="/oss/ueditor/" mapping="/oss/ueditor/**" />
<mvc:resources location="/upload/" mapping="/upload/**" />
```


## `mvc:interceptors`拦截器
```xml
<!-- 拦截器配置，拦截顺序：先执行后定义的，排在第一位的最后执行。-->
<mvc:interceptors>
<mvc:interceptor>
<mvc:mapping path="${adminPath}/**" />
<mvc:exclude-mapping path="${adminPath}/"/>
<mvc:exclude-mapping path="${adminPath}/login"/>
<mvc:exclude-mapping path="${adminPath}/sys/menu/tree"/>
<mvc:exclude-mapping path="${adminPath}/sys/menu/treeData"/>
<mvc:exclude-mapping path="${adminPath}/oa/oaNotify/self/count"/>
<bean class="com.thinkgem.jeesite.modules.sys.interceptor.LogInterceptor" />
</mvc:interceptor>
<!-- 手机视图拦截器 -->
<mvc:interceptor>
<mvc:mapping path="/**" />
<bean class="com.thinkgem.jeesite.modules.sys.interceptor.MobileInterceptor" />
</mvc:interceptor>
</mvc:interceptors>
```
```java
@RequestMapping(value = "${adminPath}/login", method = RequestMethod.GET)
```
`config.properties`
```properties
adminPath = /a
```


## 支持Shiro对Controller的方法级AOP安全控制
```xml
<!-- 支持Shiro对Controller的方法级AOP安全控制 begin-->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor">
<property name="proxyTargetClass" value="true" />
</bean>
```
## 异常控制(转发)
```xml
<!-- 异常控制(转发) -->
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
<property name="exceptionMappings">
<props>
<prop key="org.apache.shiro.authz.UnauthorizedException">error/403</prop>
<prop key="java.lang.Throwable">error/500</prop>
</props>
</property>
</bean>
```
