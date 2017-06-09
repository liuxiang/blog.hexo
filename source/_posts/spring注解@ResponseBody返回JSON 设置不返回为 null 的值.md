title:  spring注解@ResponseBody返回JSON 设置不返回为 null 的值


date: 2017-5-16 00:00:00
tags: [ spring ]



---


# 方法 一

```
<mvc:annotation-driven>
    <mvc:message-converters register-defaults="true">
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="com.fasterxml.jackson.databind.ObjectMapper">
                    <property name="serializationInclusion">
                        <value type="com.fasterxml.jackson.annotation.JsonInclude.Include">NON_NULL</value>
                    </property>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```
参考链接：

https://segmentfault.com/q/1010000002522525/a-1020000002522849

https:// stackoverflow.com/questions/4823358/spring-configure-responsebody-json-format#answer-13876694



---
# 方法 二
在你要输出的数据的类加上
```
@JsonSerialize(include=JsonSerialize.Inclusion.NON_NULL)
```
参考文章：
http://stackoverflow.com/questions/6049523/how-to-configure-spring-mvc-3-to-not-return-null-object-in-json-response#answer-6049967


---
**参考**
`spring 注解 @ResponseBody 返回JSON 设置不返回为 null 的值 - yxwb1253587469的博客 - 博客频道 - CSDN.NET`
blog.csdn.net/yxwb1253587469/article/details/71195207
