title: java8 初探——Lambda
date: 2015-12-08 00:00:01 #发表日期，一般不改动
categories: java8  #文章文类 
tags: [java8 ,Lambda ]

---


#  Lambda ForEach  

```java
package java8.lambda;


import java.util.Arrays;
import java.util.List;


public class LambdaForEach {
public static void main(String[] args) {
List<String> list = Arrays.asList("aaa", "bbb", "ccc");


// java 6,7
for (String item : list) {
System.out.println(item);
}


// java 8 Lambda
list.forEach((item) -> System.out.println(item));


// java 8 Lambda 更简
list.forEach(System.out::println);


/**
* @解析
* @default void forEach(Consumer<? super T> action)
* 
* @Consumer 定义:
* 
*           <pre>
* &#64;FunctionalInterface 
* public interface Consumer<T> 
* void accept(T t);
*           </pre>
* 
* @开始解剖:
* @1.forEach如参为Consumer
* @2.Consumer的函数接口定义为accept(T t)
* @3.System .out::println对应Consumer.accept(T t)的封装实现为
* 
*           <pre>
*           public void println(String x) {
*           synchronized (this) {
*           print(x);
*           newLine();
*           }
*           }
*           </pre>
* 
* @4.等同于
* 
* <pre>
*        list.forEach((x) -> {
*         synchronized (LambdaForEach.class) {
*         System.out.println(x);
*         }
*        });
*        </pre>
*/


// `java 8 Lambda 更简` 的翻译表达
list.forEach((x) -> {
synchronized (LambdaForEach.class) {
System.out.println(x);
}
});


// 其它,return 数组
System.out.println(list);
}
}
```


<!-- more -->



#  Lambda Sort    
```java
package java8.lambda;


import java.util.Arrays;
import java.util.Comparator;
import java.util.List;


public class LambdaSort {
public static void main(String[] args) {
String[] array = { "bbb", "ddd", "aaa" };


// java 6
Arrays.sort(array, new Comparator<String>() {
public int compare(String o1, String o2) {
return o1.compareTo(o2);
}
});
System.out.println(Arrays.asList(array));


// java 8 Lambda Array Sort
String[] array2 = { "bbb", "ddd", "aaa" };
Arrays.sort(array2, (String a, String b) -> a.compareTo(b));
System.out.println(Arrays.asList(array2));


// java 8 Lambda List Sort
List<String> list = Arrays.asList("bbb", "ddd", "aaa");
list.sort((String a, String b) -> a.compareTo(b));
System.out.println(list);
}
}
```


#  Lambda Stream
```java
package java8.lambda;


import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;


public class LambdaStream {
public static void main(String[] args) {
List<String> list = Arrays.asList("abc", "abb", "ddd");


// 逻辑过虑
List<String> listTemp = new ArrayList<String>();
list.forEach((item) -> {
if (item.startsWith("a"))
listTemp.add(item);
});
listTemp.forEach(System.out::println);


System.out.println("---");


// stream().filter 过虑函数-内容
list.stream().filter((String item) -> item.startsWith("a")).forEach(System.out::println);


System.out.println("---");

// stream().filter 过虑函数-范围
list = Arrays.asList("abc", "abb", "ddd", "111", "ddd", "yyy", "999");
list.stream().limit(5).forEach(System.out::println);


}
}
```


#  Lambda Thread 
```java
package  java8.lambda;
public   class  LambdaThread {
     public   static   void  main(String[]  args ) {
         // 1.继承Thread类
         new  hello( "A" ).start();
         new  hello( "B" ).start();
         // 2.实现Runnable接口
         new  Thread( new  helloR( "线程A" )).start();
         new  Thread( new  helloR( "线程B" )).start();
         // 3.匿名接口
        String  name  =  "匿名接口线程" ;
         new  Thread( new  Runnable() {
             @Override
             public   void  run() {
                 for  ( int   i  = 0;  i  < 5;  i ++) {
                    System. out .println( name  +  "运行     "  +  i );
                }
            }
        }).start();
         // 4.Lambda
        Runnable  runnable  = () -> System. out .println( "hello thread" );
         new  Thread( runnable ).start();
         // 5.Lambda 匿名使用
         new  Thread(() -> System. out .println( "hello thread" )).start();
         /***
         * 关键在于Runnable对象支持了函数接口`@FunctionalInterface`
         * 
         * @()无参是与`public abstract void run();`对应,所以无参数
         *  @逻辑 :()  ->  System.out.println("hello thread")为Runnable.run()函数的匿名实现
         * 
         *         <pre>
         * @FunctionalInterface
         * public interface Runnable
         *         </pre>
         * 
         */
         // 6.Lambda TwoColons 实现(即把函数的密名实现,封装了一层.方便调用且方便复用)
         new  Thread(LambdaThreadTwoColons:: run ).start();
         // 7.myRunabele New TwoColons 实现
         new  Thread(myRunabele:: new ).start();
    }
}
/**
 *  @author   Rollen - Holt  继承Thread类,直接调用run方法
 */
class  hello  extends  Thread {
     private  String  name ;
     public  hello() {
    }
     public  hello(String  name ) {
         this . name  =  name ;
    }
     public   void  run() {
         for  ( int   i  = 0;  i  < 5;  i ++) {
            System. out .println( name  +  "运行     "  +  i );
        }
    }
     // public static void main(String[]  args ) {
     // hello h1 = new hello("A");
     // hello h2 = new hello("B");
     // h1.run();
     // h2.run();
     // }
}
/**
 *  @author   Rollen - Holt  实现Runnable接口
 */
class  helloR  implements  Runnable {
     public  helloR() {
    }
     public  helloR(String  name ) {
         this . name  =  name ;
    }
     public   void  run() {
         for  ( int   i  = 0;  i  < 5;  i ++) {
            System. out .println( name  +  "运行     "  +  i );
        }
    }
     // public static void main(String[]  args ) {
     // helloR h1=new helloR("线程A");
     // Thread demo= new Thread(h1);
     // helloR h2=new helloR("线程Ｂ");
     // Thread demo1=new Thread(h2);
     // demo.start();
     // demo1.start();
     // }
     private  String  name ;
}
/**
 * LambdaThread TwoColons realize
 * 
 *  @author   liuxiang@wosaitech.com
 *  @creation  2015年12月7日
 */
class  LambdaThreadTwoColons {
     public   static   void  run() {
        System. out .println( "Lambda Thread TwoColons realize" );
    }
}
/**
 * myRunabele New TwoColons realize
 * 
 *  @author   liuxiang@wosaitech.com
 *  @creation  2015年12月7日
 */
class  myRunabele {
     public  myRunabele() {
        System. out .println( "myRunabele TwoColons realize" );
    }
}
```
# github 源码示例
https://github.com/liuxiang/myPro/tree/master/java8-example/src/java8