title: 【java脚手架搭建】容器 web.xml
date: 2017-3-8 00:00:00
tags: [ 脚手架 ]


---
# `web.xml` 相关配置


## 载入Spring配置文件(默认:*)
```xml
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath:applicationContext.xml</param-value>
</context-param>
```
或
```xml
<!-- Context ConfigLocation -->
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>classpath*:spring/spring-context*.xml</param-value>
</context-param>
```
或(`同时初始化shiro权限框架`)
```xml
<!-- Context ConfigLocation -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath*:spring/spring-context*.xml,classpath*:spring/spring-context-shiro.xml</param-value>
</context-param>
```


## Shiro过滤器(可选)
```xml
 <!-- 配置Shiro过滤器,先让Shiro过滤系统接收到的请求 -->
<!-- 这里filter-name必须对应applicationContext.xml中定义的<bean id="shiroFilter"/> -->
<!-- 使用[/*]匹配所有请求,保证所有的可控请求都经过Shiro的过滤 -->
<!-- 通常会将此filter-mapping放置到最前面(即其他filter-mapping前面),以保证它是过滤器链中第一个起作用的 -->
<filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <!-- 该值缺省为false,表示生命周期由SpringApplicationContext管理,设置为true则表示由servlet container管理 -->
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>


<filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```


## servlet `所有的业务请求由此调度(Dispatcher)`
```xml
<servlet>
<servlet-name>dispatcher</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<load-on-startup>1</load-on-startup>
</servlet>


<servlet-mapping>
<servlet-name>dispatcher</servlet-name>
<url-pattern>/</url-pattern>
</servlet-mapping>
```
此配置,默认目标是：`/WEB-INF/dispatcher-servlet.xml`
或
```xml
    <!-- servlet调度器 -->
    <servlet>
        <servlet-name>springServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath*:spring/spring-servlet*.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```


## 监听
```xml
<!-- Spring 监听启动 -->
<listener>
<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<listener>
<listener-class>org.springframework.web.context.request.RequestContextListener</listener-class>
</listener>
```


## 过滤器
- 编码
```xml
<!-- 过滤器:编码 -->
<filter>
<filter-name>SpringEncodingFilter</filter-name>
<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<init-param>
 <param-name>encoding</param-name>
 <param-value>UTF-8</param-value>
</init-param>
<init-param>
 <param-name>forceEncoding</param-name>
 <param-value>true</param-value>
</init-param>
</filter>
<filter-mapping>
<filter-name>SpringEncodingFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```


- 权限控制
```xml
<!-- 权限控制 Apache Shiro -->
<filter>
<filter-name>shiroFilter</filter-name>
<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
<init-param>
<param-name>targetFilterLifecycle</param-name>
<param-value>true</param-value>
</init-param>
</filter>
<filter-mapping>
<filter-name>shiroFilter</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```
- 跨域过滤器
```
<filter>
<filter-name>DomainFilter</filter-name>
<init-param>
<param-name>access.control.allow.origin</param-name>
<param-value>*</param-value>
</init-param>
<filter-class>com.wosai.teach.filter.DomainFilter</filter-class>
</filter>


<filter-mapping>
<filter-name>DomainFilter</filter-name>
<servlet-name>spring servlet</servlet-name>
</filter-mapping>
```
-  put patch无法接收body的参数?
```
<!-- put patch无法接收body的参数 -->
<filter>
<filter-name>httpPutFormContentFilter</filter-name>
<filter-class>org.springframework.web.filter.HttpPutFormContentFilter
</filter-class>
</filter>
<filter-mapping>
<filter-name>httpPutFormContentFilter</filter-name>
<servlet-name>spring servlet</servlet-name>
</filter-mapping>
```
# 异常响应
```xml
<!-- 异常配置 -->
<error-page>
<error-code>400</error-code>
<location>/error/400.html</location>
</error-page>
<error-page>
<error-code>404</error-code>
<location>/error/404.html</location>
</error-page>
<error-page>
<error-code>500</error-code>
<location>/error/500.html</location>
</error-page>
```


# 欢迎页
```xml
<!-- 欢迎界面 -->
<welcome-file-list>
<welcome-file>index.html</welcome-file>
</welcome-file-list>
```


