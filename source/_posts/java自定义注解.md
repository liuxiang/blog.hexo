title: java自定义注解
date: 2015-10-07 01:00:00 #发表日期，一般不改动
categories: coding #文章文类
tags: [java,注解] #文章标签，多于一项时用这种格式


---


### 获取注解-示例：
```java
Class clazz = DescriptionTest.class;
boolean d = clazz.isAnnotationPresent(Description.class)                             // 判断注解是否存在
Description desc = (Description)clazz.getAnnotation(Description.class);     // 获取注解信息
```
```java
    public static void main(String[] args) {
       Class clazz = DescriptionTest.class;
       if(clazz.isAnnotationPresent(Description.class)){
           Description desc = (Description)clazz.getAnnotation(Description.class);
           System.out.println("desc.author:" + desc.author());
           System.out.println("desc.size:" + desc.size());
       }else{
           System.out.println("没有在DescriptionTest上使用注解!");
       }
    }
```
参考：http://blog.sina.com.cn/s/blog_69a4df530100p8w3.html


| 类型 | 注解 |
| --------   | -----  |
| 面向切面编程 | @Aspect，@Pointcut等 |
| 依赖注入 | @Autowired，@Inject等 |
| 持久化 | @Entity，@Id等 |


<!-- more -->


---


## J2SE5.0版本在 java.lang.annotation提供了四种元注解，专门注解其他的注解：


```
@Documented –注解是否将包含在JavaDoc中
@Retention –什么时候使用该注解
@Target? –注解用于什么地方
@Inherited – 是否允许子类继承该注解
@Documented–一个简单的Annotations标记注解，表示是否将注解信息添加在java文档中。
@Retention– 定义该注解的生命周期。


RetentionPolicy.SOURCE – 在编译阶段丢弃。这些注解在编译结束之后就不再有任何意义，所以它们不会写入字节码。@Override, @SuppressWarnings都属于这类注解。
RetentionPolicy.CLASS – 在类加载的时候丢弃。在字节码文件的处理中有用。注解默认使用这种方式。
RetentionPolicy.RUNTIME– 始终不会丢弃，运行期也保留该注解，因此可以使用反射机制读取该注解的信息。我们自定义的注解通常使用这种方式。


@Target – 表示该注解用于什么地方。如果不明确指出，该注解可以放在任何地方。以下是一些可用的参数。需要说明的是：属性的注解是兼容的，如果你想给7个属性都添加注解，仅仅排除一个属性，那么你需要在定义target包含所有的属性。


ElementType.TYPE:用于描述类、接口或enum声明
ElementType.FIELD:用于描述实例变量
ElementType.METHOD
ElementType.PARAMETER
ElementType.CONSTRUCTOR
ElementType.LOCAL_VARIABLE
ElementType.ANNOTATION_TYPE 另一个注释
ElementType.PACKAGE 用于记录java文件的package信息


@Inherited – 定义该注释和子类的关系
```
来源： <http://www.importnew.com/10294.html>




