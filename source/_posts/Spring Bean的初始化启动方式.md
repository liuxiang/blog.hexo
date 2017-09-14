title: Spring Bean的初始化启动方式
date: 2017-08-1 00:00:00
tags: [spring]

---

# 方式一: `xml <bean init-method="init"`
```
<bean class="cn.fraudmetrix.billing.consumer.FlowConsumer" init-method="init" destroy-method="destroy">
    <property name="kafkaTopics" value="${billing.kafka.topics}"/>
    <property name="debug" value="${billing.flow.consumer.debug}"/>
</bean>
```

# 方式二: `implements InitializingBean`
```
public class DisconfPropertiesFactory implements FactoryBean<Properties>, InitializingBean {
    ...
    public void afterPropertiesSet() throws Exception {
        if (this.singleton) {
            this.singletonInstance = this.createProperties();
        }
    }
    ...
```

# 方式三:`注解@PostConstruct @PreDestroy`
```
package com.myapp.core.annotation.init;  

import javax.annotation.PostConstruct;  
import javax.annotation.PreDestroy;  

public class PersonService {  

    private String message;  

    public String getMessage() {  
        return message;  
    }  

    public void setMessage(String message) {  
        this.message = message;  
    }  

    @PostConstruct  
    public void init(){  
        System.out.println("I'm init method using @PostConstrut...."+message);  
    }  

    @PreDestroy  
    public void dostory(){  
        System.out.println("I'm destory method using @PreDestroy....."+message);  
    }  

} 
```

---
## bean factory负责bean创建的最初四步，然后移交给应用上下文做后续创建过程：
- 1.Spring初始化bean
- 2.Spring将值和其他bean的引用注入（inject）到当前bean的对应属性中；
- 3.如果Bean实现了BeanNameAware接口，Spring会传入bean的ID来调用setBeanName方法；
- 4.如果Bean实现了BeanFactoryAware接口，Spring传入bean factory的引用来调用setBeanFactory方法；
- 5.如果Bean实现了ApplicationContextAware接口，Spring将传入应用上下文的引用来调用setApplicationContext方法；
- 6.如果Bean实现了BeanPostProcessor接口，则Spring调用postProcessBeforeInitialization方法，这个方法在初始化和属性注入之后调用，在任何初始化代码之前调用；
- 7.如果Bean实现了InitializingBean接口，则需要调用该接口的afterPropertiesSet方法；如果在bean定义的时候设置了init-method属性，则需要调用该属性指定的初始化方法；
- 8.如果Bean实现了BeanPostProcessor接口，则Spring调用postProcessAfterInitialization方法
- 9.在这个时候bean就可以用于在应用上下文中使用了，当上下文退出时bean也会被销毁；
- 10.如果Bean实现了DisposableBean接口，Spring会调用destroy()方法;如果在bean定义的时候设置了destroy-method， 则此时需要调用指定的方法。

`Spring实战1：Spring初探 - 简书`
http://www.jianshu.com/p/9370707091ef

---
**参考**
`13 Spring Bean init-method 和 destroy-method实例 - 简书`
http://www.jianshu.com/p/30a0a74cd31f

`通过Spring @PostConstruct 和 @PreDestroy 方法 实现初始化和销毁bean之前进行的操作 - 安德里亚的成长 - CSDN博客`
http://blog.csdn.net/topwqp/article/details/8681497
