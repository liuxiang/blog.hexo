title: 【java脚手架搭建】事务 spring-context-transaction.xml
date: 2017-3-8 00:00:06
tags: [ 脚手架 ]


---


# 事务相关
- `transactionManager`配置事务管理器(定义事务) 
注:ref="dataSource"不在上文,需要合并到`spring-context.xml`中配置
```xml
<!-- 定义事务 -->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="dataSource" />
</bean>
```


- 事务设置
```xml
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="select*" read-only="true"/>
        <tx:method name="find*" read-only="true"/>
        <tx:method name="get*" read-only="true"/>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>
```
配置参考
```properties
propagation 不 REQUIRED 事务传播行为
isolation 不 DEFAULT 事务隔离级别
timeout 不 -1 事务超时的时间（以秒为单位）
read-only 不 false 事务是否只读？
rollback-for 不   将被触发进行回滚的 Exception(s)；以逗号分开。 如：'com.foo.MyBusinessException,ServletException'
no-rollback-for 不   不 被触发进行回滚的 Exception(s)；以逗号分开。 如：'com.foo.MyBusinessException
```


- 参与事务方法
```xml
<aop:config>
    <aop:pointcut id="appService" expression="execution(* com.isea533.mybatis.service..*Service*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="appService"/>
</aop:config>
```


---
**支持**
`spring tx:advice事务配置 - rong_wz的专栏 - 博客频道 - CSDN.NET`
http://blog.csdn.net/rong_wz/article/details/53787648
