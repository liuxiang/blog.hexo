title: java8 初探——Optional
date: 2015-12-08 00:00:05 #发表日期，一般不改动
categories: java8 #文章文类
tags: [java8, Optional]
 
---


# MyOptional
```java
package java8.optional;
import java.util.Optional;
public class MyOptional {
    public static void optional() {
        Optional<String> optional = Optional.of("bam");
        optional.isPresent(); // true
        optional.get(); // "bam"
        optional.orElse("fallback");// "bam"
        optional.ifPresent((s) -> System.out.println(s.charAt(0)));// "b"
    }
    public static void optionalMore() {
        String str = "aaa";
        Optional<String> optional_2 = Optional.of(str); // 如果str ==
                                                        // null，抛出错误NullPointerException
        Optional<String> optional = Optional.ofNullable(str); // 如果str ==
                                                                // null，返回一个空Optional<String>
        Optional.<String> empty(); // 返回一个空Optional<String>
        String s = optional.get(); // 获取被包装的值
        optional.ifPresent((value) -> System.out.println("hello " + value)); // 如果optional的value不是null，则执行函数表达式
        optional.orElse("elseValue"); // 如果optional的value为null，则返回"elseValue"
        optional.orElseGet(() -> "orElseGet"); // 如果optional的value不是null，则返回函数表达式的执行结果
        optional.orElseThrow(RuntimeException::new); // 如果optional的value不是null，则抛出错误
        optional.filter((value) -> value.length() == 5); // 过滤得到长度等于5的value
        optional.map((value) -> {
            System.out.println("map:" + value);
            return value;
        });
        optional.flatMap((value) -> {
            System.out.println("flatMap:" + value);
            return Optional.ofNullable(value);
        });
        // map 与 flatMap 区别
        Optional<String> os = Optional.of("bbb");
        os.map((value) -> Optional.of(value)); // 返回的类型是Optional<Optional<String>>
        os.flatMap((value) -> Optional.of(value)); // 返回的类型是Optional<String>
    }
    public static void main(String[] args) {
        optional();
        optionalMore();
    }
}
/**
* Optionals(可选项)
*
* @Optionals是没有函数的接口，取而代之的是防止 NullPointerException
 *                             异常。这是下一节的一个重要概念，所以让我们看看如何结合Optionals工作。
*
* @Optional is a simple container for a value which may be null or non-null.
 *           Think of a method which may return a non-null result but sometimes
 *           return nothing. Instead of returning null you return an Optional in
 *           Java 8.
*
* @Optional是一个简单的容器，这个值可能是空的或者非空的。考虑到一个方法可能会返回一个non-null的值， 也可能返回一个空值。
 *                                                           为了不直接返回null，我们在Java
 *                                                           8中就返回一个Optional。
*
*
* @http://www.tuicool.com/articles/RjqEJrm
**/
```
 
<!-- more -->
 
# MyOptionalExample
```java
package java8.optional;
import java.util.Optional;
public class MyOptionalExample {
    private void nameNotNull() {
        String name = "Tony";
        Optional<String> optName = Optional.of(name);
        System.out.println(optName.get());
    }
    private void nameNull() {
        String name = null;
        Optional<String> optName = Optional.of(name);// of method make NPE Boom
        System.out.println(optName.get());
    }
    private void nameNullable() {
        String name = null;
        Optional<String> optName = Optional.ofNullable(name);
        System.out.println(optName.get());// get method make
                                            // NoSuchElementException: No value
                                            // present
    }
    private void nameIsPresent() {
        String name = null;
        Optional<String> optName = Optional.ofNullable(name);
        if (optName.isPresent()) {
            System.out.println(optName.get());
        } else {
            System.out.println("Name is null.");
        }
    }
    private void nameEmpty() {
        String name = null;
        Optional<String> optName = (name == null) ? Optional.empty() : Optional.of(name);
        System.out.println(optName.get());// get method make
                                            // NoSuchElementException: No value
                                            // present
    }
    private void nameOrElse() {
        String name = null;
        Optional<String> optName = Optional.ofNullable(name);
        System.out.println(optName.orElse("Name is null."));
    }
    private void nameOrElseGet() {
        String name = null;
        Optional<String> optName = Optional.ofNullable(name);
        System.out.println(optName.orElseGet(() -> "WHAT! null!"));// lambda
    }
    private void nameOrElseThrow() {
        class MyException extends Exception {
            public MyException(String message) {
                super(message);
            }
        }
        String name = null;
        Optional<String> optName = Optional.ofNullable(name);
        try {
            System.out.println(optName.orElseThrow(() -> new MyException("WHAT! NULL!")));
        } catch (MyException e) {
            System.out.println("MY EXCEPTION! " + e.getMessage());
        }
    }
    public static void main(String[] args) {
        new MyOptionalExample().nameNotNull();
        new MyOptionalExample().nameNull();
        new MyOptionalExample().nameNullable();
        new MyOptionalExample().nameIsPresent();
        new MyOptionalExample().nameEmpty();
        new MyOptionalExample().nameOrElse();
        new MyOptionalExample().nameOrElseGet();
        new MyOptionalExample().nameOrElseThrow();
    }
}
/**
* @Java 8 - Optional
*
* @Optional 是值的容器，只有兩種狀態，不是有值就是沒值。目的是做為 null 的替代方案。Optional 提供工廠方法，將你輸入的值產生為
* @Optional 物件，這時Optional 物件即為該值的容器，若要取回該值，必須使用 get() 方法。
* @Optional 屬於 value-based ，對於識別敏感的操作(包含參考相等的判斷(==)、hash
 *           code或同步(synchronization)等)會有不確定性的結果，應該要避免使用這些操作。
*
* @http://www.tuicool.com/articles/ERBzIrE
*/
```
 
# user
```java
package java8.optional;
 
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
 
class User {
    public int id;
    public String name;
 
    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }
 
    // ==== 不使用 Optional ====
    private List<User> getUsers() {
        // 假裝從資料庫取值
        List<User> users = Arrays.asList(new User(1, "Tony"), new User(2, "John"), new User(3, "Emma"));
        return users;// 有可能 null
    }
 
    private User findUserByName(String name) {
        List<User> users = getUsers();
        if (users != null) {// 很好，有檢查 null
            for (User u : users) {
                if (u.name.equals(name)) {
                    return u;
                }
            }
        }
        return null;// 可能找不到而返回 null
    }
 
    private void optionalDemo() {
        User user = findUserByName("Amy");
        if (user != null) {// 很好，有檢查 null
            System.out.println(user.name + " id is " + user.id);
        } else {
            System.out.println("User not found.");
        }
    }
 
    // ==== 使用 Optional ====
    private Optional<List<User>> getUsers2() {
        // 假裝從資料庫取值
        List<User> users = Arrays.asList(new User(1, "Tony"), new User(2, "John"), new User(3, "Emma"));
        return Optional.ofNullable(users);
    }
 
    private Optional<User> findUserByName2(String name) {
        Optional<List<User>> users = getUsers2();
        // 對要使用的資料檢查，參數也要
        if (users.isPresent() && Optional.ofNullable(name).isPresent()) {
            // 使用 stream 取代 foreach 迴圈
            // findFirst()回傳的是 Optional
            return users.get().stream().filter(u -> u.name.equals(name)).findFirst();
        }
        return Optional.empty();
    }
 
    private void optionalDemo2() {
        Optional<User> user = findUserByName2("Amy");
        // 使用 isPresent() 有點冗長
        if (user.isPresent()) {
            System.out.println(user.get().name + " id is " + user.get().id);
        } else {
            System.out.println("User not found.");
        }
        // 改用 ifPresent 只要一行，但是無值時不做任何回應
        user.ifPresent(u -> System.out.println(u.name + " id is " + u.id));
    }
}
```
 
# github 源码示例
https://github.com/liuxiang/myPro/tree/master/java8-example/src/java8
 
---
 
**code1**
```
public void sayHello(String name){
    if(name==null){
        name = "游客";
    }
    System.out.println("Hello, "+name);
}
```
 
**code2**
```
import com.google.common.base.Optional;
 
public void sayHello(String name){
    name = Optional.fromNullable(name).or("游客");
    System.out.println("Hello, "+name);
}
```
 
**参考**
`使用Optional避免NullPointerException`
http://www.tuicool.com/articles/uIzeYjf