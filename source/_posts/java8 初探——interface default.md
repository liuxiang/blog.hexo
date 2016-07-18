title: java8 初探——interface default
date: 2015-12-08 00:00:02 #发表日期，一般不改动
categories: java8 #文章文类
tags: [java8, interface default]
 
---
# interface default
```java
package java8.interfaceDefault;
public class Formula {
    public static void main(String[] args) {
        FormulaI formula = new FormulaI() {
            @Override
            public double calculate(int a) {
                return sqrt(a * 100);
            }
        };
 
        // 自定义实现
        System.out.println(formula.calculate(100));// 100.0
        System.out.println(formula.sqrt(16));// 4.0
 
        // Lambda实现
        System.out.println(((FormulaI) (a) -> (a * 100)).calculate(4));// 400.0
    }
}
@FunctionalInterface
interface FormulaI {
 
    double calculate(int a);
    /**
     * java 8 为接口增加了默认函数实体,且不要求继承实现
     *
     * @param a
     * @return
     */
    default double sqrt(int a) {
        return Math.sqrt(a);
    }
} 
```
<!-- more -->
# github 源码示例
https://github.com/liuxiang/myPro/tree/master/java8-example/src/java8