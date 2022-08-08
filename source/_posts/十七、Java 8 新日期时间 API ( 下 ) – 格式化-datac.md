---
title: 十七、Java 8 新日期时间 API ( 下 ) – 格式化
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 1054668a
date: 2022-05-05 19:09:32
updated: 2022-05-18 18:49:16
---

# 十七、Java 8 新日期时间 API ( 下 ) – 格式化

Java 8 似乎也对 `java.text.SimpleDateFormat` 也不太满意，竟然重新创建了一个 `java.time.format` 包，该包下包含了几个类和枚举用于格式化日期时间。

## java.time.format 包

`java.time.format` 包提供了以下几个类用于格式化日期时间

|            类            |                  说明                  |
| :----------------------: | :------------------------------------: |
|    DateTimeFormatter     | 用于打印和解析日期时间对象的格式化程序 |
| DateTimeFormatterBuilder |     创建日期时间格式化样式的构建器     |
|       DecimalStyle       | 日期和时间格式中使用的本地化十进制样式 |

`java.time.format` 包还提供了以下几个枚举，包含了常见的几种日期时间格式。

|     枚举      |                        说明                        |
| :-----------: | :------------------------------------------------: |
|  FormatStyle  | 包含了本地化日期，时间或日期时间格式器的样式的枚举 |
| ResolverStyle |        包含了解决日期和时间的不同方法的枚举        |
|   SignStyle   |         包含了如何处理正/负号的方法的枚举          |
|   TextStyle   |          包含了文本格式和解析的样式的枚举          |

## DateTimeFormatter 类

`DateTimeFormatter` 类格式化日期时间的最重要的类，该类是一个最终类，只能实例化，不能被扩展和继承。

`DateTimeFormatter` 类的定义如下

```JAVA
public final class DateTimeFormatter extends Object
```

`DateTimeFormatter` 类用于打印和解析日期时间对象的格式化器。此类提供打印和解析的主要应用程序入口点，并提供 DateTimeFormatter 的常见模式

- 使用预定义的常量，比如 `ISO_LOCAL_DATE`
- 使用模式字母，例如 `uuuu-MMM-dd`
- 使用本地化样式，例如 `long` 或 `medium`

所有的日期时间类，包括本地日期时间和包含时区的日期时间类，都提供了两个重要的方法

1、 一个用于格式化，`format(DateTimeFormatter formatter)`
2、 另一个用于解析，`parse(CharSequence text, DateTimeFormatter formatter)`

下面，我们写几个示例来演示下这两个方法，并演示下如和使用 `DateTimeFormatter` 类

#### Java8Tester

```JAVA
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZonedDateTime now = ZonedDateTime.now();
      System.out.println("当前时间是: " + now);

      System.out.println("另一种表示形式:" + now.format(DateTimeFormatter.RFC_1123_DATE_TIME));
   }
}
```

运行结果如下

```
当前时间是: 2022-05-05T11:47:57.680725Z[Etc/UTC]
另一种表示形式:Thu, 5 May 2022 11:47:57 GMT
```

我们还可以调用 `DateTimeFormatter.ofPattern()` 方法创建自己的日期时间格式，例如

```JAVA
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZonedDateTime now = ZonedDateTime.now();
      System.out.println("当前时间是: " + now);

      DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd H:m:s");
      System.out.println("另一种表示形式:" + now.format(formatter));
   }
}
```

运行结果如下

```
当前时间是: 2022-05-05T11:48:37.111221Z[Etc/UTC]
另一种表示形式:2022/05/05 11:48:37

```

当然了，我们可以调用 `LocalDateTime` 类的静态方法 `parse()` 将我们刚刚自定义的日期时间格式给解析回来

```JAVA
import java.time.LocalDateTime;
import java.time.ZonedDateTime;
import java.time.format.DateTimeFormatter;

public class Java8Tester {

   public static void main(String args[]) {
      Java8Tester tester = new Java8Tester();
      tester.run();
   }

   public void run() {

      ZonedDateTime now = ZonedDateTime.now();
      System.out.println("当前时间是: " + now);

      DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd H:m:s");
      String text = now.format(formatter);
      System.out.println("另一种表示形式:" + text );

      LocalDateTime parsed = LocalDateTime.parse(text, formatter);
      System.out.println("解析后:" + parsed );
   }
}
```

运行结果如下

```
当前时间是: 2022-05-05T11:49:07.577534Z[Etc/UTC]
另一种表示形式:2022/05/05 11:49:7
解析后:2022-05-05T11:49:07
```