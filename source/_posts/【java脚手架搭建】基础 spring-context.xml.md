title: 【java脚手架搭建】基础 spring-context.xml
date: 2017-3-8 00:00:02
tags: [ 脚手架 ]


---
# `applicationContext.xml`或`spring-context.xml`相关配置


## 属性文件载入
```xml
<context:property-placeholder location="classpath:config.properties"/>
```


## 数据库连接相关
- `dataSource`数据源配置, 不使用连接池
```xml
<!-- 数据源配置, 不使用连接池 -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
<property name="driverClassName" value="${jdbc.driver}" />
<property name="url" value="${jdbc.url}" />
<property name="username" value="${jdbc.username}"/>
<property name="password" value="${jdbc.password}"/>
</bean>
```
- `dataSource`数据源配置, 使用 BoneCP 数据库连接池
```xml
<!-- 数据源配置, 使用 BoneCP 数据库连接池 -->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close"> 
    <!-- 数据源驱动类可不写，Druid默认会自动根据URL识别DriverClass -->
    <property name="driverClassName" value="${jdbc.driver}" />
    
<!-- 基本属性 url、user、password -->
<property name="url" value="${jdbc.url}" />
<property name="username" value="${jdbc.username}" />
<property name="password" value="${jdbc.password}" />

<!-- 配置初始化大小、最小、最大 -->
<property name="initialSize" value="${jdbc.pool.init}" />
<property name="minIdle" value="${jdbc.pool.minIdle}" /> 
<property name="maxActive" value="${jdbc.pool.maxActive}" />

<!-- 配置获取连接等待超时的时间 -->
<property name="maxWait" value="60000" />

<!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
<property name="timeBetweenEvictionRunsMillis" value="60000" />

<!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
<property name="minEvictableIdleTimeMillis" value="300000" />

<property name="validationQuery" value="${jdbc.testSql}" />
<property name="testWhileIdle" value="true" />
<property name="testOnBorrow" value="false" />
<property name="testOnReturn" value="false" />

<!-- 打开PSCache，并且指定每个连接上PSCache的大小（Oracle使用）
<property name="poolPreparedStatements" value="true" />
<property name="maxPoolPreparedStatementPerConnectionSize" value="20" /> -->

<!-- 配置监控统计拦截的filters -->
    <property name="filters" value="stat" /> 
</bean>
```
- `jndi-lookup id="dataSource"`数据源配置, 使用应用服务器的数据库连接池
```xml
<!-- 数据源配置, 使用应用服务器的数据库连接池 -->
<jee:jndi-lookup id="dataSource" jndi-name="java:comp/env/jdbc/jeesite" />
```
- `*Mapper.xml` 映射生效配置
```xml
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="basePackage" value="io.renren.dao" />
</bean>
```


---
## MyBatis相关
-  MyBatis 初始化`[Mybatis-Spring]applicationContext.xml`
```xml
<!-- MyBatis 初始化 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
<property name="dataSource" ref="dataSource"/>
<property name="mapperLocations" value="classpath:mapper/*.xml"/>
<property name="typeAliasesPackage" value="com.wosai.bright.model"/>
<property name="plugins">
<array>
<bean class="com.github.pagehelper.PageInterceptor">
<!-- 这里的几个配置主要演示如何使用，如果不理解，一定要去掉下面的配置 -->
<property name="properties">
<value>
helperDialect=mysql
reasonable=true
supportMethodsArguments=true
params=count=countSql
autoRuntimeDialect=true
</value>
</property>
</bean>
</array>
</property>
</bean>
```
或 `[jeesite]spring-context.xml`
```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
<property name="dataSource" ref="dataSource"/>
<property name="typeAliasesPackage" value="com.thinkgem.jeesite"/>
<property name="typeAliasesSuperType" value="com.thinkgem.jeesite.common.persistence.BaseEntity"/>
<property name="mapperLocations" value="classpath:/mapper/**/*.xml"/>
<property name="configLocation" value="classpath:mybatis/mybatis-config.xml"></property>
</bean>
```
组合`mybatis/mybatis-config.xml`
```xml
<settings>


</settings>


<!-- 类型别名 -->
<typeAliases>
<typeAlias alias="Page" type="com.thinkgem.jeesite.common.persistence.Page" /><!--分页  -->
</typeAliases>


<!-- 插件配置 -->
<plugins>
<plugin interceptor="com.thinkgem.jeesite.common.persistence.interceptor.PaginationInterceptor" />
</plugins>
```
- `Mapper<Entity>`类支持(注解)
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
或
```xml
<!-- 扫描basePackage下所有以@MyBatisDao注解的接口 -->
<bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
<property name="basePackage" value="com.wosai.bright.mapper"/>
</bean>
```


## 定义事务
```xml
<!-- 定义事务 -->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" scope="prototype">
<constructor-arg index="0" ref="sqlSessionFactory"/>
</bean>


<aop:aspectj-autoproxy/>


<aop:config>
<aop:pointcut id="appService" expression="execution(* com.wosai.bright.service..*Service*.*(..))"/>
<aop:advisor advice-ref="txAdvice" pointcut-ref="appService"/>
</aop:config>


<tx:advice id="txAdvice" transaction-manager="transactionManager">
<tx:attributes>
<tx:method name="select*" read-only="true"/>
<tx:method name="find*" read-only="true"/>
<tx:method name="get*" read-only="true"/>
<tx:method name="*"/>
</tx:attributes>
</tx:advice>


<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
<property name="dataSource" ref="dataSource"/>
</bean>
```


- 配置 Annotation 驱动，扫描@Transactional注解的类定义事务
```xml
<!-- 配置 Annotation 驱动，扫描@Transactional注解的类定义事务  -->
<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
```


---
## 校验器
```xml
<!-- 配置 JSR303 Bean Validator 定义 -->
<bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean" />
```


## 计划任务
```xml
<!-- 计划任务配置，用 @Service @Lazy(false)标注类，用@Scheduled(cron = "0 0 2 * * ?")标注方法 -->
<task:executor id="executor" pool-size="10"/> 
<task:scheduler id="scheduler" pool-size="10"/>
<task:annotation-driven scheduler="scheduler" executor="executor" proxy-target-class="true"/>
```


# 代理相关
- 激活自动代理功能
```xml
<!-- 激活自动代理功能 -->
<aop:aspectj-autoproxy proxy-target-class="true" /> 
```


# 主动引入
(不一定需要，可在web.xml中通配引入.如：`spring-content*.xml`)
```xml
<!-- 主动引入 -->
<import resource="classpath:spring/spring-servlet.xml"/> <!-- 调度 -->
<import resource="classpath:spring/spring-context-annotation.xml"/> <!-- 注解 -->
<import resource="classpath:spring/spring-context-shiro.xml"/> <!-- 权限 -->
```
