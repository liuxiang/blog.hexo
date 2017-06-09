title: 【java脚手架搭建】权限 spring-context-shiro.xml
date: 2017-3-8 00:00:04
tags: [ 脚手架 ]


---


# [renren-security]spring-shiro.xml ```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:mvc="http://www.springframework.org/schema/mvc"
xsi:schemaLocation="
        http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans-4.2.xsd        
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.2.xsd
        http://www.springframework.org/schema/tx 
      http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
      http://www.springframework.org/schema/aop 
      http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
http://www.springframework.org/schema/mvc 
      http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd">


<!-- 继承自AuthorizingRealm的自定义Realm,即指定Shiro验证用户登录的类为自定义的UserRealm.java -->  
<bean id="userRealm" class="io.renren.shiro.UserRealm"/>

<!-- Shiro默认会使用Servlet容器的Session,可通过sessionMode属性来指定使用Shiro原生Session -->  
<!-- 即<property name="sessionMode" value="native"/>,详细说明见官方文档 -->  
<!-- 这里主要是设置自定义的单Realm应用,若有多个Realm,可使用'realms'属性代替 -->  
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">  
   <property name="realm" ref="userRealm"/>
</bean>

<!-- Shiro主过滤器本身功能十分强大,其强大之处就在于它支持任何基于URL路径表达式的、自定义的过滤器的执行 -->  
<!-- Web应用中,Shiro可控制的Web请求必须经过Shiro主过滤器的拦截,Shiro对基于Spring的Web应用提供了完美的支持 -->  
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">  
   <!-- Shiro的核心安全接口,这个属性是必须的 -->  
   <property name="securityManager" ref="securityManager"/>  
   <!-- 要求登录时的链接(可根据项目的URL进行替换),非必须的属性,默认会自动寻找Web工程根目录下的"/login.html"页面 -->  
   <property name="loginUrl" value="/login.html"/>  
   <!-- 登录成功后要跳转的连接 -->  
   <property name="successUrl" value="/index.html"/>
   <!-- 用户访问未对其授权的资源时,所显示的连接 -->  
   <!-- 若想更明显的测试此属性可以修改它的值,如unauthor.jsp,然后用[玄玉]登录后访问/admin/listUser.jsp就看见浏览器会显示unauthor.jsp -->  
   <property name="unauthorizedUrl" value="/"/>  
   <!-- Shiro连接约束配置,即过滤链的定义 -->  
   <!-- 此处可配合我的这篇文章来理解各个过滤连的作用http://blog.csdn.net/jadyer/article/details/12172839 -->  
   <!-- 下面value值的第一个'/'代表的路径是相对于HttpServletRequest.getContextPath()的值来的 -->  
   <!-- anon：它对应的过滤器里面是空的,什么都没做,这里.do和.jsp后面的*表示参数,比方说login.jsp?main这种 -->  
   <!-- authc：该过滤器下的页面必须验证后才能访问,它是Shiro内置的一个拦截器org.apache.shiro.web.filter.authc.FormAuthenticationFilter -->  
   <property name="filterChainDefinitions">  
       <value>
        /statics/**=anon
        /login.html=anon
        /sys/login=anon
        /captcha.jpg=anon
        /**=authc
       </value>
   </property>
</bean>

<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

<!-- AOP式方法级权限检查  -->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor">
<property name="proxyTargetClass" value="true" />
</bean>
<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
    <property name="securityManager" ref="securityManager"/>
</bean>
</beans>
```


# [jeesite]spring-content-shiro.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context-4.1.xsd"
default-lazy-init="true">


<description>Shiro Configuration</description>


    <!-- 加载配置属性文件 -->
<context:property-placeholder ignore-unresolvable="true" location="classpath:jeesite.properties" />

<!-- Shiro权限过滤过滤器定义 -->
<bean name="shiroFilterChainDefinitions" class="java.lang.String">
<constructor-arg>
<value>
/static/** = anon
/userfiles/** = anon
${adminPath}/cas = cas
${adminPath}/login = authc
${adminPath}/logout = logout
${adminPath}/** = user
/act/editor/** = user
/ReportServer/** = user
</value>
</constructor-arg>
</bean>

<!-- 安全认证过滤器 -->
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
<property name="securityManager" ref="securityManager" /><!-- 
<property name="loginUrl" value="${cas.server.url}?service=${cas.project.url}${adminPath}/cas" /> -->
<property name="loginUrl" value="${adminPath}/login" />
<property name="successUrl" value="${adminPath}?login" />
<property name="filters">
            <map>
                <entry key="cas" value-ref="casFilter"/>
                <entry key="authc" value-ref="formAuthenticationFilter"/>
            </map>
        </property>
<property name="filterChainDefinitions">
<ref bean="shiroFilterChainDefinitions"/>
</property>
</bean>

<!-- CAS认证过滤器 -->  
<bean id="casFilter" class="org.apache.shiro.cas.CasFilter">  
<property name="failureUrl" value="${adminPath}/login"/>
</bean>

<!-- 定义Shiro安全管理配置 -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
<property name="realm" ref="systemAuthorizingRealm" />
<property name="sessionManager" ref="sessionManager" />
<property name="cacheManager" ref="shiroCacheManager" />
</bean>

<!-- 自定义会话管理配置 -->
<bean id="sessionManager" class="com.thinkgem.jeesite.common.security.shiro.session.SessionManager"> 
<property name="sessionDAO" ref="sessionDAO"/>

<!-- 会话超时时间，单位：毫秒  -->
<property name="globalSessionTimeout" value="${session.sessionTimeout}"/>

<!-- 定时清理失效会话, 清理用户直接关闭浏览器造成的孤立会话   -->
<property name="sessionValidationInterval" value="${session.sessionTimeoutClean}"/>
<!--   <property name="sessionValidationSchedulerEnabled" value="false"/> -->
  <property name="sessionValidationSchedulerEnabled" value="true"/>
 
<property name="sessionIdCookie" ref="sessionIdCookie"/>
<property name="sessionIdCookieEnabled" value="true"/>
</bean>

<!-- 指定本系统SESSIONID, 默认为: JSESSIONID 问题: 与SERVLET容器名冲突, 如JETTY, TOMCAT 等默认JSESSIONID,
当跳出SHIRO SERVLET时如ERROR-PAGE容器会为JSESSIONID重新分配值导致登录会话丢失! -->
<bean id="sessionIdCookie" class="org.apache.shiro.web.servlet.SimpleCookie">
   <constructor-arg name="name" value="jeesite.session.id"/>
</bean>


<!-- 自定义Session存储容器 -->
<!-- <bean id="sessionDAO" class="com.thinkgem.jeesite.common.security.shiro.session.JedisSessionDAO"> -->
<!-- <property name="sessionIdGenerator" ref="idGen" /> -->
<!-- <property name="sessionKeyPrefix" value="${redis.keyPrefix}_session_" /> -->
<!-- </bean> -->
<bean id="sessionDAO" class="com.thinkgem.jeesite.common.security.shiro.session.CacheSessionDAO">
<property name="sessionIdGenerator" ref="idGen" />
<property name="activeSessionsCacheName" value="activeSessionsCache" />
<property name="cacheManager" ref="shiroCacheManager" />
</bean>

<!-- 自定义系统缓存管理器-->
<!-- <bean id="shiroCacheManager" class="com.thinkgem.jeesite.common.security.shiro.cache.JedisCacheManager"> -->
<!-- <property name="cacheKeyPrefix" value="${redis.keyPrefix}_cache_" /> -->
<!-- </bean> -->
<bean id="shiroCacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
<property name="cacheManager" ref="cacheManager"/>
</bean>

<!-- 保证实现了Shiro内部lifecycle函数的bean执行 -->
<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

<!-- AOP式方法级权限检查  -->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor">
<property name="proxyTargetClass" value="true" />
</bean>
<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
    <property name="securityManager" ref="securityManager"/>
</bean>

</beans>
```