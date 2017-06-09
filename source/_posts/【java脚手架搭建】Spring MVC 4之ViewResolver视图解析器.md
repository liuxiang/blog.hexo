title: 【java脚手架搭建】Spring MVC 4之ViewResolver视图解析器
date: 2017-3-9 00:00:02
tags: [ 脚手架 ]


---


# 定义视图文件解析
```
<!-- 定义视图文件解析 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="${web.view.prefix}"/>
<property name="suffix" value="${web.view.suffix}"/>
</bean>
```
```
web.view.prefix=/WEB-INF/views/
web.view.suffix=.jsp
```
或
```
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/views/" />
<!-- 取消自动后缀,程序主动填写能同时使用.jsp和.html的文件 -->
<!--<property name="suffix" value=".jsp" />-->
</bean>
```
以及`[renren-security]spring-mvc.xml`
```
<!-- Velocity视图解析器    默认视图  -->
<bean id="velocityViewResolver" class="org.springframework.web.servlet.view.velocity.VelocityViewResolver">
<property name="contentType" value="text/html;charset=UTF-8" />
<property name="viewNames" value="*.html" />
<property name="suffix" value=""/>
<property name="dateToolAttribute" value="date" />
<property name="numberToolAttribute" value="number" /> 
<property name="toolboxConfigLocation" value="/WEB-INF/velocity-toolbox.xml" />
<property name="requestContextAttribute" value="rc"/>
        <!--第一个匹配的是freemarker的视图解析器，如果匹配不成功，则自动选择order=1的其他解析器--> 

<property name="order" value="0"/>
</bean>


<bean id="velocityConfigurer" class="org.springframework.web.servlet.view.velocity.VelocityConfigurer">
<property name="resourceLoaderPath" value="/WEB-INF/page/" />
<property name="velocityProperties">
  <props>
<prop key="input.encoding">UTF-8</prop>
<prop key="output.encoding">UTF-8</prop>
<prop key="contentType">text/html;charset=UTF-8</prop>
  </props>
</property>
</bean>


<!-- JSP视图解析器  -->
<bean id="viewResolverJsp" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
<property name="prefix" value="/WEB-INF/page/"/>
<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
<property name="viewNames" value="*.jsp" />
<property name="suffix" value=""/>
<property name="order" value="1"/>
</bean>


<!-- FreeMarker视图解析器 -->
<bean id="viewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">
<property name="viewClass" value="org.springframework.web.servlet.view.freemarker.FreeMarkerView"/>
<property name="contentType" value="text/html; charset=utf-8"/>
<property name="cache" value="false"/>
<property name="viewNames" value="*.ftl" />
<property name="suffix" value=""/>
<property name="order" value="2"/>
</bean>
<bean class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
<property name="templateLoaderPath" value="/WEB-INF/page/"/>
</bean>
```
注意：InternalResourceViewResolver必须总是赋予最低的优先级（最大的order值），因为不管返回什么视图名称，它都将解析视图。如果它的优先级高于其它解析器的优先级的话，它将使得其它具有较低优先级的解析器没有机会解析视图。



`Spring MVC视图解析器：配置多个视图解析器的优先级`
http://www.cnblogs.com/rollenholt/archive/2012/12/27/2836035.html


`SpringMVC多视图解析器(jsp,html,title解析器) - zhuzhoulin的博客 - 博客频道 - CSDN.NET`

http://blog.csdn.net/zhuzhoulin/article/details/52371530



```
<bean id="velocityViewResolver" class="org.springframework.web.servlet.view.velocity.VelocityLayoutViewResolver">  
    <!-- 每个页面都引用 -->  
    <property name="exposeRequestAttributes" value="true" />  
    <!-- 页面类型 -->  
    <property name="contentType" value="text/html;charset=UTF-8" />  
    <!-- 前缀 -->  
    <property name="prefix" value="" />  
    <!-- 后缀 -->  
    <property name="suffix" value=".html" />  
    <!-- 每个页面都引用 -->  
    <property name="layoutUrl" value="layout.html" />  
    <!-- 当前项目域名(IP)加端口号,html页面引用${rc.contextPath}-->  
    <property name="requestContextAttribute" value="rc" />  
    <!--第一个匹配的是freemarker的视图解析器，如果匹配不成功，则自动选择order=1的其他解析器-->  
    <property name="order" value="0" />  
</bean>  
<bean id="velocityConfigurer" class="org.springframework.web.servlet.view.velocity.VelocityConfigurer">  
    <property name="resourceLoaderPath">  
        <value>WEB-INF/pages</value>  
    </property>  
    <property name="velocityProperties">  
        <props>  
            <prop key="input.encoding">UTF-8</prop>  
            <prop key="output.encoding">UTF-8</prop>  
            <prop key="contentType">text/html;charset=UTF-8</prop>  
        </props>  
    </property>  
</bean>  
<!--jsp视图解析器 -->  
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
    <property name="prefix" value="/WEB-INF/" />  
    <property name="suffix" value=".jsp" />  
    <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />  
        <property name="order" value="1" />  
</bean>
```
`springMVC配置html和jsp视图 - 紫色的魅力 - 博客频道 - CSDN.NET`
http:// blog.csdn.net/u012012240/article/details/51082964


`SpringMVC同时支持多视图(JSP,Velocity,Freemarker等)的一种思路实现 - 一片相思林 - 博客园`
http://www.cnblogs.com/huligong1234/p/3515747.html


---


```
`AbstractCachingViewResolver`：用来缓存视图的抽象视图解析器。通常情况下，视图在使用前就准备好了。继承改解析器就能够使用视图缓存。


`XmlViewResolver`：XML视图解析器。它实现了ViewResolver接口，接受相同DTD定义的XML配置文件作为Spring的XML bean工厂。


`ResourceBundleViewResolver`：它使用了ResourceBundle定义下的bean，实现了ViewResolver接口，指定了绑定包的名称。通常情况下，配置文件会定义在classpath下的properties文件中，默认的文件名字是views.properties。


`UrlBasedViewResolver`：它简单实现了ViewResolver接口，它不用显式定义，直接影响逻辑视图到URL的映射。它让你不用任何映射就能通过逻辑视图名称访问资源。


`InternalResourceViewResolver`：国际化视图解析器。


`VelocityViewResolver /FreeMarkerViewResolver`：Velocity或FreeMarker视图解析器。


`ContentNegotiatingViewResolve`：内容谈判视图解析器
```


`Spring MVC 4之ViewResolver视图解析器 - 小尘 - 博客频道 - CSDN.NET`
http://blog.csdn.net/shehun1/article/details/43984877