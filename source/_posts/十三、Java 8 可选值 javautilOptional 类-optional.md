---
title: 十三、Java 8 可选值 java.util.Optional 类
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 3b70afd5
date: 2022-05-05 19:03:48
updated: 2022-05-18 18:48:30
---

# 十三、Java 8 可选值 java.util.Optional 类

在不考虑竖起来的情况下，抛一个硬币，落地时，显示正面的情况只有两种：是正面和不是正面。很多时候，这是一个 「 谓词 」，也就是返回布尔类型 ( bool )。但有时候，我们需要返回另一种类型：存在 和 空。

- **存在** 就是硬币落地时显示为正面
- **空** 就是硬币落地式显示的不是正面。

从另一方面说，结果就是 **有值** 和 **空** 。

一个类，如果可以同时表示 **有值** 和 **空** ，我们称这种类为 **可选类** ( Optional )

从某些方面说，`Optional` 类型就是 「那里有一个值，它等于 x，或者那里没有那个值」

## JAVA 8 java.util.Optional 类

Java 8 在 `java.util` 包中添加了一个新的类 `Optional` 。

`Optional` 类是一个容器，用于表示可能包含也可能不包含非 null 值。如果存在值，`isPresent()` 方法将返回 `true`，`get()` 将返回该值。

`Optional` 类提供了许多方法用于处理 「 可用 」 或 「 不可用 」 ，而不是简单的检查空值情况。

`java.util.Optional` 类的声明如下

```
public final class Optional<T> extends Object
```

> 注意：该类是一个最终类，不能被继承和扩展。

## `Optional` 类提供了以下静态方法来创建 Optional 类的实例

```
Optional` 类提供了三个静态方法用于创建 `Optional` 类的实例，这三个方法的返回值都是 `Optional<T>
```

|        方法         |                             说明                             |
| :-----------------: | :----------------------------------------------------------: |
|       empty()       |          创建一个空 ( empty ) 的 Optional 类的实例           |
|     of(T value)     |   创建一个包含了指定 `T` 类型的 `value` 值的 Optional 实例   |
| ofNullable(T value) | 如果 `value` 非 `null` ，则创建一个包含了指定 `T` 类型的 `value` 值的 Optional 实例，否则创建一个空的 Optional 实例 |

## `Optional` 类提供的方法

|                          方法/说明                           |
| :----------------------------------------------------------: |
| **boolean equals(Object obj)** 判断某个其它的对象是否 「 等于 」 此 Optional |
| **Optional<T>s; filter(Predicate<? super T> predicate)** 如果存在值，并且值与给定谓词匹配，则返回描述值的 Optional，否则返回空 Optional |
| **<U> Optional<U&gst; flatMap(Function<? super T,Optional<U>> mapper)** 如果值存在，则将 `map` 应用到该值上并返回应用后的结果，如果值不存在，则返回一个空的 Optional |
| **T get()** 如果此 Optional 中存在值，则返回该值，否则抛出`NoSuchElementException` 异常 |
| **int hashCode()** 如果值存在，则返回当前值的哈希值，如果不存在值，则返回 `0` |
| **void ifPresent(Consumer<? super T> consumer)** 如果值存在，则使用该值作为参数调用方法 `consumer` 。如果值不存在，则什么事情都不做 |
| **boolean isPresent()** 如果值存在则返回 `true` ，否则返回 `false` |
| **<U> Optional<U> map(Function<? super T,? extends U> mapper)** 如果存在值，则将传递的 `map` 函数应用于该值，如果结果为非 null，则返回描述结果的 Optionals |
|  **T orElse(T other)** 如果值存在则返回值，否则返回 `other`  |
| **T orElseGet(Supplier<? extends T> other)** 如果值存在则返回值，否则调用 `other` 并返回该调用的结果 |
| **<X extends Throwable> T orElseThrow(Supplier<? extends X>>exceptionSupplier)** 如果值存在，则返回包含的值，否则抛出由开发者提供的异常 |
| **String toString()** 返回此 Optional 的非空字符串表示形式，一般用于调试 |

## 范例

我们写一个范例演示下 Optional 类的使用

#### OptionalTester.java

```JAVA
import java.util.Optional;

public class OptionalTester
{

   public static void main(String args[]) {
      OptionalTester tester = new OptionalTester();
      Integer value1 = null;
      Integer value2 = Integer.valueOf(10);

      //Optional.ofNullable - allows passed parameter to be null.
      Optional<Integer> a = Optional.ofNullable(value1);

      //Optional.of - throws NullPointerException if passed parameter is null
      Optional<Integer> b = Optional.of(value2);
      System.out.println(tester.sum(a,b));
   }

   public Integer sum(Optional<Integer> a, Optional<Integer> b)
   {
      //Optional.isPresent - checks the value is present or not

      System.out.println("First parameter is present: " + a.isPresent());
      System.out.println("Second parameter is present: " + b.isPresent());

      //Optional.orElse - returns the value if present otherwise returns
      //the default value passed.
      Integer value1 = a.orElse(Integer.valueOf(0));

      //Optional.get - gets the value, value should be present
      Integer value2 = b.get();
      return value1 + value2;
   }
}
```

运行以上范例，输出结果如下

```
First parameter is present: false
Second parameter is present: true
10
```

