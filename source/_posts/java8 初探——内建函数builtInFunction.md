title: java8 初探——内建函数builtInFunction
date: 2015-12-08 00:00:06 #发表日期，一般不改动
categories: java8  #文章文类
tags: [ java8 ,   builtInFunction ]


---


#  BuiltInFunction
```java
package java8.builtInFunction;


import java.time.Clock;
import java.time.DayOfWeek;
import java.time.Instant;
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.Month;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.ChronoUnit;
import java.util.ArrayList;
import java.util.Comparator;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Locale;
import java.util.Map;
import java.util.Objects;
import java.util.Optional;
import java.util.UUID;
import java.util.concurrent.TimeUnit;
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;


public class BuiltInFunction {


public static void predicate() {
Predicate<String> predicate = (s) -> s.length() > 0;


System.out.println(predicate.test("foo"));// true
System.out.println(predicate.negate().test("foo"));// false


Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;


Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();
}


public static void function() {
Function<String, Integer> toInteger = Integer::valueOf;
Function<String, String> backToString = toInteger.andThen(String::valueOf);


System.out.println(backToString.apply("123"));// "123"
}


public static void supplier() {
Supplier<Person> personSupplier = Person::new;
personSupplier.get(); // new Person
}


public static void consumer() {
Consumer<Person> greeter = (p) -> System.out.println("Hello, " + p.firstName);
greeter.accept(new Person("Luke", "Skywalker"));
}


public static void comparator() {
Comparator<Person> comparator = (p1, p2) -> p1.firstName.compareTo(p2.firstName);


Person p1 = new Person("John", "Doe");
Person p2 = new Person("Alice", "Wonderland");


comparator.compare(p1, p2); // > 0
comparator.reversed().compare(p1, p2); // < 0
}


public static void optional() {
Optional<String> optional = Optional.of("bam");


optional.isPresent(); // true
optional.get(); // "bam"
optional.orElse("fallback");// "bam"


optional.ifPresent((s) -> System.out.println(s.charAt(0)));// "b"
}


public static void stream() {
List<String> stringCollection = new ArrayList<String>();
stringCollection.add("ddd2");
stringCollection.add("aaa2");
stringCollection.add("bbb1");
stringCollection.add("aaa1");
stringCollection.add("bbb3");
stringCollection.add("ccc");
stringCollection.add("bbb2");
stringCollection.add("ddd1");


// stringCollection.stream().filter((String item) ->
// item.startsWith("a")).forEach(System.out::println);
stringCollection.stream().filter((s) -> s.startsWith("a")).forEach(System.out::println);


stringCollection.stream().sorted().filter((s) -> s.startsWith("a")).forEach(System.out::println);
System.out.println(stringCollection);


stringCollection.stream().map(String::toUpperCase).sorted((a, b) -> b.compareTo(a)).forEach(System.out::println);


boolean anyStartsWithA = stringCollection.stream().anyMatch((s) -> s.startsWith("a"));
System.out.println(anyStartsWithA); // true


boolean allStartsWithA = stringCollection.stream().allMatch((s) -> s.startsWith("a"));
System.out.println(allStartsWithA); // false


boolean noneStartsWithZ = stringCollection.stream().noneMatch((s) -> s.startsWith("z"));
System.out.println(noneStartsWithZ); // tr


long startsWithB = stringCollection.stream().filter((s) -> s.startsWith("b")).count();
System.out.println(startsWithB); // 3


Optional<String> reduced = stringCollection.stream().sorted().reduce((s1, s2) -> s1 + "#" + s2);
reduced.ifPresent(System.out::println); // "aaa1#aaa2#bbb1#bbb2#bbb3#ccc#ddd1#ddd2"
}


/**
* 并行流
*/
public static void parallelFlow() {
int max = 1000000;
List<String> values = new ArrayList<>(max);
for (int i = 0; i < max; i++) {
UUID uuid = UUID.randomUUID();
values.add(uuid.toString());
}


/** 串行排序 */
long t0 = System.nanoTime();


long count = values.stream().sorted().count();
System.out.println(count);


long t1 = System.nanoTime();


long millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("sequential sort took: %d ms", millis));
// sequential sort took: 899 ms


/** 并行排序 */
t0 = System.nanoTime();


count = values.parallelStream().sorted().count();
System.out.println(count);


t1 = System.nanoTime();


millis = TimeUnit.NANOSECONDS.toMillis(t1 - t0);
System.out.println(String.format("parallel sort took: %d ms", millis));


// parallel sort took: 472 ms
}


public static void map() {
// map是不支持流操作的
Map<Integer, String> map = new HashMap<>();


for (int i = 0; i < 10; i++) {
map.putIfAbsent(i, "val" + i);
}


map.forEach((id, val) -> System.out.println(val));


map.computeIfPresent(3, (num, val) -> val + num);
map.get(3);// val33


map.computeIfPresent(9, (num, val) -> null);
map.containsKey(9);// false


map.computeIfAbsent(23, num -> "val" + num);
map.containsKey(23);// true


map.computeIfAbsent(3, num -> "bam");
map.get(3);// val33


map.remove(3, "val3");
map.get(3);// val33


map.remove(3, "val33");
map.get(3);// null


map.getOrDefault(42, "not found");// not found


map.merge(9, "val9", (value, newValue) -> value.concat(newValue));
map.get(9);// val9


map.merge(9, "concat", (value, newValue) -> value.concat(newValue));
map.get(9);// val9concat
}


public static void dateApi() {
Clock clock = Clock.systemDefaultZone();
long millis = clock.millis();


Instant instant = clock.instant();
Date legacyDate = Date.from(instant); // legacy java.util.Date


// 时区
System.out.println(ZoneId.getAvailableZoneIds());
// prints all available timezone ids


ZoneId zone1 = ZoneId.of("Europe/Berlin");
ZoneId zone2 = ZoneId.of("Brazil/East");
System.out.println(zone1.getRules());
System.out.println(zone2.getRules());


// ZoneRules[currentStandardOffset=+01:00]
// ZoneRules[currentStandardOffset=-03:00]


// localTime 比较
LocalTime now1 = LocalTime.now(zone1);
LocalTime now2 = LocalTime.now(zone2);


System.out.println(now1.isBefore(now2)); // false


long hoursBetween = ChronoUnit.HOURS.between(now1, now2);
long minutesBetween = ChronoUnit.MINUTES.between(now1, now2);


System.out.println(hoursBetween); // -3
System.out.println(minutesBetween); // -239


LocalTime late = LocalTime.of(23, 59, 59);
System.out.println(late); // 23:59:59


// 时间字符串进行解析
DateTimeFormatter germanFormatter = DateTimeFormatter.ofLocalizedTime(FormatStyle.SHORT).withLocale(Locale.GERMAN);


LocalTime leetTime = LocalTime.parse("13:37", germanFormatter);
System.out.println(leetTime); // 13:37


// LocalDate
LocalDate today = LocalDate.now();
LocalDate tomorrow = today.plus(1, ChronoUnit.DAYS);
LocalDate yesterday = tomorrow.minusDays(2);


LocalDate independenceDay = LocalDate.of(2014, Month.JULY, 4);
DayOfWeek dayOfWeek = independenceDay.getDayOfWeek();
System.out.println(dayOfWeek); // FRIDAY


// 解析字符串并形成LocalDate对象
germanFormatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.MEDIUM).withLocale(Locale.GERMAN);


LocalDate xmas = LocalDate.parse("24.12.2014", germanFormatter);
System.out.println(xmas); // 2014-12-24
}


public static void main(String[] args) {
predicate();// 断言
function();// 功能函数
consumer();//
comparator();//
optional();
stream();
}
}


class Person {
String firstName;
String lastName;


Person() {
}


Person(String firstName, String lastName) {
this.firstName = firstName;
this.lastName = lastName;
}
}


/**
 * Java8 简明教程 - 推酷
 * 
 * @http://www.tuicool.com/articles/buuIjeB
 **/


```
<!-- more -->

# github 源码示例
https://github.com/liuxiang/myPro/tree/master/java8-example/src/java8