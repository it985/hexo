---
title: 十六、Java 8 新日期时间 API ( 中 ) – 时区日期时间
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 65f9a50c
date: 2022-05-05 19:08:20
updated: 2022-05-18 18:49:01
---

# 十六、Java 8 新日期时间 API ( 中 ) – 时区日期时间

上一章节 Java 8 新日期时间 API ( 上 ) – 本地日期时间 我们对 Java 8 重新设计的日期时间 API 做了一些基础的介绍，同时详细介绍了和本地时间有关的几个类 `LocalDateTime` 、`LocalDate` 和 `LocalTime` 。

我同时也发现，这三个类没有任何时区相关的信息，但也不能说它们没处理时区，而只能说它们有选择的隐藏了时区的处理。它们内部会使用操作系统当前的时区。

以此同时，Java 在 `java.time` 包中也提供了几个类用于处理需要关注时区的日期时间 API。它们是 `java.time.ZonedDateTime` 和 `java.time.ZoneId`。前者用于处理需要时区的日期时间，后者用于处理时区。

`ZonedDateTime` 和 `LocalDateTime` 类似，几乎有着相同的 API。从某些方面说，`ZonedLocalTime` 如果不传递时区信息，那么它会默认使用操作系统的时区，这样，结果其实和 `LocalDateTime` 是类似的。

比如，我们可以使用 `ZonedDateTime` 的 `now()` 方法返回当前时区 ( 操作系统时区 ) 的日期时间，调用 `parse()` 方法可以将一个包含了时区信息的字符串格式的日期时间转化为一个 ZonedDateTime 实例。

#### Java8Tester.java

```JAVA
import java.time.ZonedDateTime;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZonedDateTime now = ZonedDateTime.now();
      System.out.println("当前日期时间是:" + now);

      ZonedDateTime datetime = ZonedDateTime.parse("2022-12-12T21:58:20+02:00");
      System.out.println("日期时间是:" + datetime);
   }
}
```

运行结果如下

```
当前日期时间是:2022-05-05T11:45:25.602159Z[Etc/UTC]
日期时间是:2022-12-12T21:58:20+02:00
```

我们还可以调用 `ZonedDateTime` 对象的 `toLocalDate()` 和 `toLocalTime()` 方法将获取该实例的转换为本地的日期和时间

```JAVA
import java.time.LocalTime;
import java.time.LocalDate;
import java.time.ZonedDateTime;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZonedDateTime now = ZonedDateTime.now();
      System.out.println("当前日期时间是:" + now);

      LocalDate date = now.toLocalDate();
      System.out.println("本地日期是:" + now);

      LocalTime time = now.toLocalTime();
      System.out.println("本地时间是:" + time);
   }
}
```

运行结果如下

```
当前日期时间是:2022-05-05T11:46:41.247743Z[Etc/UTC]
本地日期是:2022-05-05T11:46:41.247743Z[Etc/UTC]
本地时间是:11:46:41.247743
```

> P.S 这个返回值好诡异，为啥 `toLocalDate()` 还返回了时间？

## 处理时区

时区相关的信息，我们可以使用 `ZoneId` 类来处理。

比如可以调用 `ZoneId` 类的静态方法 `systemDefault()` 返回当前的时区。

```JAVA
import java.time.ZonedDateTime;
import java.time.ZoneId;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZoneId currentZone = ZoneId.systemDefault();
      System.out.println("当前时区是: " + currentZone);
   }
}
```

运行结果如下

```
当前时区是: Asia/Shanghai
```

我们还可以调用 `ZonedDateTime` 实例的 `getZone()` 方法获取实例所在的时区

```JAVA
import java.time.ZonedDateTime;
import java.time.ZoneId;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZonedDateTime now = ZonedDateTime.now();
      System.out.println("当前时区是: " + now.getZone());
   }
}
```

运行结果如下

```
当前时区是: Asia/Shanghai
```