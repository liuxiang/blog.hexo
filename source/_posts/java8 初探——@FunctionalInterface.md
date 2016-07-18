

title: java8 初探——@FunctionalInterface
date: 2015-12-08 00:00:03 #发表日期，一般不改动
categories: java8 #文章文类
tags: [java8, FunctionalInterface]
 
---


# functionalInterface
```java
package java8.functionalInterface;
public class Func {
    public static void main(String[] args) {
        FuncI func = (String first, String last) -> {
            return first + "." + last;
        };
        System.out.println(func.builder("f", "l"));
        /**
         * @我的理解:依据函数接口`FuncI`的定义
         * @指明定义(String first, String last)
         * @使用快捷实现: [-> first + "." + last],同时调用,响应
         * @像匿名函數(方法),最终的响应对象是FuncI(@FunctionalInterface)
         */
        func = (String first, String last) -> first + "." + last;
        System.out.println(func.builder("f", "l"));
 
        // 关系对应 FuncI <-> (String first, String last) -> first + "." + last | 对象的具体匿名实现
        System.out.println(((FuncI)(String first, String last) -> first + "." + last).builder("f", "l"));
    }
}
@FunctionalInterface
interface FuncI {
    String builder(String firstName, String lastName);
 
    /**
     * @错误提示
     * <pre>
     * Invalid '@FunctionalInterface' annotation; FuncI is not a functional interface
     * 无效的@FunctionalInterface注释;FuncI不是一个功能接口
     * </pre>
     */
    // String builder2(String firstName, String lastName);
} 
```
<!-- more -->
# github 源码示例
https://github.com/liuxiang/myPro/tree/master/java8-example/src/java8