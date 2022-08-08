---
title: 十五、Java 8 新日期时间 API ( 上 ) – 本地日期时间
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: a5516f38
date: 2022-05-05 19:07:12
updated: 2022-05-18 18:48:40
---

# 十五、Java 8 新日期时间 API ( 上 ) – 本地日期时间

作为开发者，经常需要处理日期时间。如果你跟随者 Java 5 一路走来，那么一定会对 `java.util.Date` 、`java.util.Calendar` 、`java.util.GregoiranCalendar` 和 `java.text.SimpleDateFormat` 四大类非常熟悉，它们分别用于处理日期、日历、日历表示、日期时间格式化。

这四个类，对于编程老人来讲，应该是习惯了，但对于编程新人来讲，就有好多疑问，有好多陷阱和坑等着它们跳，比如

1、 **非线程安全**：`java.util.Date` 并不是线程安全的。开发者在使用这个类时必须自己处理多线程并发问题。
2、 **设计不佳** ：一方面日期和日期格式化分布在多个包中。另一方面，`java.util.Date` 的默认日期，年竟然是从 `1900` 开始，月从 `1` 开始，日从 `0` 开始，没有统一性。而且 `Date` 类也缺少直接操作日期的相关方法。
3、 **时区处理困难**：因为设计不佳，开发人员不得不编写大量代码来处理时区问题。
4、 还有其它一些问题

面对种种问题，Java 8 终于重新设计了所有日期时间、日历及时区相关的 API。并把它们都统一放置在 `java.time` 包和子包下。并作出了以下改进

1、 新的日期时间 API 是线程安全的。不仅没有 `setter` 方法，而且任何对实例的变更都会返回一个新的实例而保证原来的实例不变。
2、 新的日期时间 API 提供了大量的方法，用于修改日期时间的各个部分，并返回一个新的实例。
3、 在时区方面，新的日期时间 API 引入了 **域** ( domain ) 这个概念。

同时 Java 8 还针对原来复杂的 API 进行重新组合和拆分，分成了好多个类。本章接下来的章节，我们就来详细介绍其中几个最重要的。

## 本地日期时间 API

Java 8 为处理本地的日期时间提供了三个类 `LocalDate` 、`LocalTime` 和 `LocalDateTime`。分别用于处理 **本地日期**、**本地时间** 和 **本地日期时间**。

当使用这三个类时，开发者并不需要关心时区是什么。因为它默认使用的是操作系统的时区。

比如，可以使用 `LocalDateTime.now()` 方法返回当前的日期时间。

#### Java8Tester.java

```JAVA
import java.time.LocalDateTime;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {
      LocalDateTime currentTime = LocalDateTime.now();
      System.out.println("当前日期时间: " + currentTime);
   }
}
```

运行结果如下

```
当前日期时间: 2022-05-05T11:40:19.601459
```

比如，我们可以调用 `LocalDateTime` 对象的 `toLocalDate()` 方法和 `toLocalTime()` 分别返回当前的日期和当前的时间，也就是 `LocalDate` 和 `LocalTime` 两个类的实例

```JAVA
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {
      LocalDateTime currentTime = LocalDateTime.now();
      System.out.println("当前日期时间: " + currentTime);

      LocalDate date1 = currentTime.toLocalDate();
      System.out.println("当前日期: " + date1);

      LocalTime time1 = currentTime.toLocalTime();
      System.out.println("当前时间: " + time1);
   }
}
```

运行结果如下

```
当前日期时间: 2022-05-05T11:40:58.340724
当前日期: 2022-05-05
当前时间: 11:40:58.340724

```

比如我们可以调用 `LocalDateTime` 对象的 `getMonth()` 方法返回当前的月份，调用 `getDayOfMonth()` 返回当前的日期，调用 `getSecond()` 返回当前时间的秒数

```JAVA
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;
import java.time.Month;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {
      LocalDateTime currentTime = LocalDateTime.now();
      System.out.println("当前日期时间: " + currentTime);

      Month month = currentTime.getMonth();
      System.out.println("当前月份: " + month);

      int day = currentTime.getDayOfMonth();
      System.out.println("当前月中的第几天: " + day);

      int seconds = currentTime.getSecond();
      System.out.println("当前秒数: " + seconds);
   }
}
```

运行结果如下

```
当前日期时间: 2022-05-05T11:41:34.887247
当前月份: MAY
当前月中的第几天: 5
当前秒数: 34
```

比如我们可以调用 `LocalDateTime` 对象的 `withDayOfMonth()` 修改日并返回一个新的实例，调用 `withYear()` 修改年，调用其它 `with*` 方法修改其它属性。

这些 `with` 方法都是返回新的实例，而原来的实例并不会改变。

```JAVA
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;
import java.time.Month;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {
      LocalDateTime currentTime = LocalDateTime.now();
      System.out.println("当前日期时间: " + currentTime);

      LocalDateTime date2 = currentTime.withDayOfMonth(10).withYear(2012);
      System.out.println("新的日期时间: " + date2);

      System.out.println("原来的日期时间: " + currentTime);
   }
}
```

运行结果如下

```
当前日期时间: 2022-05-05T11:42:06.589161
新的日期时间: 2012-05-10T11:42:06.589161
原来的日期时间: 2022-05-05T11:42:06.589161
```

可以发现原先的实例并没有被修改。

同时，新的日期时间 API 还大量引入了 `of()` 方法，比如我们可以调用 `LocalDate.of()` 方法创建一个日期实例，调用 `LocalTime.of()` 方法创建一个时间实例。

```JAVA
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;
import java.time.Month;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {
      LocalDate date = LocalDate.of(2023, Month.OCTOBER, 01);
      System.out.println("日期是: " + date);
      LocalTime time = LocalTime.of(22, 15);
      System.out.println("时间是: " + time);
   }
}
```

运行结果如下

```
日期是: 2023-10-01
时间是: 22:15
```

我们还可以调用 `LocalDateTime.parse()` 、`LocalDate.parse()` 和 `LocalTime.parse()` 方法解析字符串格式的日期时间、日期和时间。

```JAVA
import java.time.LocalDate;
import java.time.LocalTime;
import java.time.LocalDateTime;
import java.time.Month;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      LocalDateTime datetime = LocalDateTime.parse("2022-12-12T21:58:00");
      System.out.println("日期时间是:" + datetime);

      LocalDate date = LocalDate.parse("2022-12-12");
      System.out.println("日期是: " + date);

      LocalTime time = LocalTime.parse("21:58:01");
      System.out.println("时间是: " + time);
   }
}
```

运行结果如下

```
日期时间是:2022-12-12T21:58
日期是: 2022-12-12
时间是: 21:58:01
```