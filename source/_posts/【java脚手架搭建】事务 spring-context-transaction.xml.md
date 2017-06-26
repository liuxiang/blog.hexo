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
```
1. ISOLATION_DEFAULT： 这是一个PlatfromTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别,另外四个与JDBC的隔离级别相对应  
2. ISOLATION_READ_UNCOMMITTED： 这是事务最低的隔离级别，它充许令外一个事务可以看到这个事务未提交的数据,这种隔离级别会产生脏读，不可重复读和幻像读。  
3. ISOLATION_READ_COMMITTED： 保证一个事务修改的数据提交后才能被另外一个事务读取。另外一个事务不能读取该事务未提交的数据  
4. ISOLATION_REPEATABLE_READ： 这种事务隔离级别可以防止脏读，不可重复读。但是可能出现幻像读,它除了保证一个事务不能读取另一个事务未提交的数据外，还保证了避免下面的情况产生(不可重复读)。  
5. ISOLATION_SERIALIZABLE 这是花费最高代价但是最可靠的事务隔离级别。事务被处理为顺序执行,除了防止脏读，不可重复读外，还避免了幻像读。   
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
