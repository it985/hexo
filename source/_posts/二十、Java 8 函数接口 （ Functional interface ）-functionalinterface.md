---
title: 二十、Java 8 函数接口 （ Functional interface ）
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 783096c
date: 2022-05-05 19:13:55
updated: 2022-05-18 18:49:34
---

# 二十、Java 8 函数接口 （ Functional interface ）

Java 8 引入了 「 函数接口 」 ( funtional interface ) 的概念，「 函数接口 」就是那些有且只有显式定义一个方法的接口。

例如，具有单个方法 `compareTo()` 的接口 `Comparable` 接口，它只有一个功能，就是用于比较。

这种函数接口一般用于 Java 8 中的 Lambda 表达式 。 而且 Java 8 为了支持 Lambda 表达式，更是定义了许多函数接口。这些接口基本都在 `java.util.function` 包中。

## 函数接口

函数接口为 Java 8 Lambda 表达式和方法引用提供目标类型。每个函数接口都有一个 虚 ( abstract ) 方法，成为该函数接口的函数方法。用于适配该类型的 Lambda 表达式的参数类型和返回值类型。

函数接口可以在多个上下文中提供目标类型，例如赋值上下文，方法调用或强制转换上下文。

我们写一小段代码演示下

```JAVA
// Assignment context
Predicate<String> p = String::isEmpty;

// Method invocation context
stream.filter(e -> e.getSize() > 10)...

// Cast context
stream.map((ToIntFunction) e -> e.getSize())..
```

## 函数接口的规则

那么，什么样的接口才能称之为函数接口呢 ？

如果一个接口的实现类只需要实现一个方法，那么该接口就是函数接口。

具体来说，有以下两种情况

1、 那些只有一个方法的接口，例如 `Comparable` 接口，它只有一个方法 `compareTo()`。
2、 那些具有多个默认方法，但有且只有一个虚方法的接口。也就是说，函数接口也可以有多个方法，但除了一个可用 Lambda 表达式来实现的方法，其它方法都必须有 `default` 关键字。

## java.util.function 包中定义的函数接口

`java.util.function` 包中定义了大量的函数接口，下表列出了这些接口，并对其做一些简单的介绍。

|          接口           |                             说明                             |
| :---------------------: | :----------------------------------------------------------: |
|     BiConsumer<T,U>     |      表示接受两个不同类型的参数，但不返回任何结果的操作      |
|    BiFunction<T,U,R>    |  表示接受两个不同类型的参数，并返回一个其它类型的结果的操作  |
|    BinaryOperator<T>    |  表示接受两个相同类型的参数，并返回一个同一类型的结果的操作  |
|    BiPredicate<T,U>     |    表示接受两个不同诶行的参数，且返回布尔类型的结果的操作    |
|     BooleanSupplier     |        不接受任何参数，且返回一个布尔类型的结果的操作        |
|       Consumer<T>       |           表示接受一个参数，但不返回任何结果的操作           |
|  DoubleBinaryOperator   | 表示接受两个 double 类型的参数，并返回 double 类型结果的操作 |
|     DoubleConsumer      |    表示接受一个 double 类型的参数，但不返回任何结果的操作    |
|    DoubleFunction<R>    | 表示接受一个 double 类型的参数，且返回一个 R 类型的结果的操作 |
|     DoublePredicate     | 表示一个接受两个 double 类型的参数， 且返回一个布尔类型的结果的操作 |
|     DoubleSupplier      |      表示一个不接受任何参数，但返回布尔类型的结果的操作      |
|   DoubleToIntFunction   | 表示接受两个 double 类型的参数，但返回一个 int 类型的结果的操作 |
|  DoubleToLongFunction   | 表示接受两个 double 类型的参数，但返回一个 long 类型的结果的操作 |
|   DoubleUnaryOperator   | 表示接受一个 double 类型的参数，且返回一个 double 类型的结果的操作 |
|      Function<T,R>      |    表示一个接受 T 类型的参数，且返回一个 R 类型结果的函数    |
|    IntBinaryOperator    | 表示一个接受两个 int 类型的参数，且返回一个 int 类型的结果的操作 |
|       IntConsumer       |     表示接受一个 int 类型的参数，但不返回任何结果的操作      |
|     IntFunction<R>      |  表示接受一个 int 类型的参数，但返回一个 R 类型的结果的操作  |
|      IntPredicate       |   表示接受一个 int 类型的参数，但返回布尔类型的结果的操作    |
|       IntSupplier       |     表示不接受任何参数，但返回一个 int 类型的结果的操作      |
|   IntToDoubleFunction   | 表示接受一个 int 类型的参数，但返回一个 double 类型的结果的操作 |
|    IntToLongFunction    | 表示接受一个 int 类型的参数，但返回一个 long 类型的结果的操作 |
|    IntUnaryOperator     | 表示接受一个 int 类型的参数，且返回一个 int 类型的结果的操作 |
|   LongBinaryOperator    | 表示接受两个 long 类型的参数，且返回一个 long 类型的结果的操作 |
|      LongConsumer       |     表示不接受任何参数，但返回一个 long 类型的结果的操作     |
|     LongFunction<R>     | 表示接受一个 loing 类型的参数，但返回一个 R 类型的结果的操作 |
|      LongPredicate      |   表示接受一个 long 类型的参数，但返回布尔类型的结果的操作   |
|      LongSupplier       |     表示不接受任何参数，但返回一个 long 类型的结果的操作     |
|  LongToDoubleFunction   | 表示接受一个 long 类型的参数，但返回一个 double 类型的结果的函数 |
|    LongToIntFunction    |  表示接受一个 long 类型的参数，但返回 int 类型的结果的函数   |
|    LongUnaryOperator    | 表示接受一个 long 类型的参数，并返回一个 long 类型的结果的操作 |
|  ObjDoubleConsumer<T>   | 表示接受两个参数，一个为 T 类型的对象，另一个 double 类型，但不返回任何结果的操作 |
|    ObjIntConsumer<T>    | 表示接受两个参数，一个为 T 类型的对象，另一个 int 类型，但不返回任何结果的操作 |
|   ObjLongConsumer<T>    | 表示接受两个参数，一个为 T 类型的对象，另一个 double 类型，但不返回任何结果的操作 |
|      Predicate<T>       |  表示接受一个指定类型 T 的参数，但返回布尔类型的结果的操作   |
|       Supplier<T>       |      表示不接受任何参数，但返回一个 T 类型的结果的操作       |
| ToDoubleBiFunction<T,U> | 表示接受两个不同类型的参数，但返回一个 double 类型的结果的操作 |
|   ToDoubleFunction<T>   | 表示一个接受指定类型 T 的参数，并返回一个 double 类型的结果的操作 |
|  ToIntBiFunction<T,U>   | 表示接受两个不同类型的参数，但返回一个 int 类型的结果的操作  |
|    ToIntFunction<T>     | 表示一个接受指定类型 T 的参数，并返回一个 int 类型的结果的操作 |
|  ToLongBiFunction<T,U>  | 表示接受两个不同类型的参数，但返回一个 long 类型的结果的操作 |
|    ToLongFunction<T>    | 表示一个接受指定类型的参数，并返回一个 long 类型的结果的操作 |
|    UnaryOperator<T>     |    表示接受一个参数，并返回一个与参数类型相同的结果的操作    |

看起来很多接口功能都是重复的。但实际上并非如此，因为各个接口表面上看起来一样，但实际上它们有着不同的默认方法

## 范例

我们写一个范例来演示下 `Predicate<T>` 函数接口的使用。

`Predicate<T>` 只有一个虚方法 `test(Object)`，该方法接受一个 T 类型的对象，然后返回布尔类型的结果。因此，我们的 Lambda 表达是的参数也是 T 类型，返回值则是布尔类型

#### Java8Tester.java

```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class Java8Tester {

   public static void main(String args[]) {
      List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);

      // Predicate<Integer> predicate = n -> true
      // n is passed as parameter to test method of Predicate interface
      // test method will always return true no matter what value n has.

      System.out.println("Print all numbers:");

      //pass n as parameter
      eval(list, n->true);

      // Predicate<Integer> predicate1 = n -> n%2 == 0
      // n is passed as parameter to test method of Predicate interface
      // test method will return true if n%2 comes to be zero

      System.out.println("Print even numbers:");
      eval(list, n-> n%2 == 0 );

      // Predicate<Integer> predicate2 = n -> n > 3
      // n is passed as parameter to test method of Predicate interface
      // test method will return true if n is greater than 3.

      System.out.println("Print numbers greater than 3:");
      eval(list, n-> n > 3 );
   }

   public static void eval(List<Integer> list, Predicate<Integer> predicate) {

      for(Integer n: list) {

         if(predicate.test(n)) {
            System.out.println(n + " ");
         }
      }
   }
}
```

运行结果如下

```
Print all numbers:
1 
2 
3 
4 
5 
6 
7 
8 
9 
Print even numbers:
2 
4 
6 
8 
Print numbers greater than 3:
4 
5 
6 
7 
8 
9
```

