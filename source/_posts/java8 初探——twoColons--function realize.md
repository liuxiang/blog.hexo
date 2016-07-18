

title: java8 初探——twoColons::function realize
date: 2015-12-08 00:00:04 #发表日期，一般不改动
categories: java8 #文章文类
tags: [java8, twoColons::function realize]
 
---


# twoColons::
```java
package java8.twoColons;
class TwoColonsUtil {
    public static String getSomething(String a, String b) {
        return a + ";" + b;
    }
    public String doGet(String a, String b) {
        return a + ";" + b;
    }
    public static void main(String[] args) {
 
        // lambda 实现
        FuncI f0 = (String a,String b) -> a + ";" + b;
        System.out.println(f0.get("aaa", "bbb"));
 
        /**
         * @我的理解:依据函数接口`FuncI`的定义 ,进行函数实现的封装
         * @获取方法的实现: TwoColonsUtil.getSomething(String a,String b)
         * @等同于:(String a,String b) -> a + ";" + b
         * @再使用快捷调用:TwoColonsUtil::getSomething 完成响应
         */
        FuncI f = TwoColonsUtil::getSomething;
        System.out.println(f.get("aaa", "bbb"));
 
        // 证明`TwoColonsUtil::getSomething`对应对象
        System.out.println(((FuncI)TwoColonsUtil::getSomething).get("aaa", "bbb"));
 
        // 非静态
        FuncI f2 = new TwoColonsUtil()::doGet;
        System.out.println(f2.get("ccc", "ddd"));
        System.out.println(((FuncI)new TwoColonsUtil()::doGet).get("ccc", "ddd"));
    }
}
@FunctionalInterface
interface FuncI {
    public String get(String a, String b);
}
```
 
# TwoColons New 构造实现
```java
package java8.twoColons;
class TwoColonsNew {
    public String name;
    public TwoColonsNew(String name) {
        this.name = name;
        // System.out.println(this.name);
    }
    public static void main(String[] args) {
        TwoColonsInterface si = TwoColonsNew::new;
        TwoColonsNew twoC = si.build("loader");
        System.out.println(twoC.name);
    }
}
@FunctionalInterface
interface TwoColonsInterface {
    // 返回构造对象
    TwoColonsNew build(String name);
}
```
<!-- more -->
# github 源码示例
https://github.com/liuxiang/myPro/tree/master/java8-example/src/java8