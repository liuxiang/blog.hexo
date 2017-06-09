title: SpringMVC返回JSON
date: 2017-3-20 00:00:00
tags: [spring ]


---


# 配置
```

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
- 关键配置
```
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
```


# 使用(关键`@ResponseBody`)
```
@RequestMapping(method = RequestMethod.GET, value = {"sysConfig"})
@ResponseBody
public List sysConfig(HttpServletRequest request, HttpServletResponse response) {
    List<SysConfig> sysConfigList = sysConfigService.selectByExample(new Example(SysConfig.class));
    return sysConfigList;
}
```
或
```
@RequestMapping(method = RequestMethod.GET, value = {"sysConfig2"})
public @ResponseBody List sysConfig2(HttpServletRequest request, HttpServletResponse response) {
    List<SysConfig> sysConfigList = sysConfigService.selectByExample(new Example(SysConfig.class));
    return sysConfigList;
}
```


---
**参考**
`配置SpringMVC返回JSON遇到的坑`
http://blog.csdn.net/caiwenfeng_for_23/article/details/43492973


`maven与springMVC之HttpMessageConverter解析json - CSDN.NET`
http://blog.csdn.net/w2865673691/article/details/48466563
