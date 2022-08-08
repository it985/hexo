---
title: 十八、Java 8 流 Stream ( 上 )
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 62e87c35
date: 2022-05-05 19:10:52
updated: 2022-05-18 18:49:21
---

# 十八、Java 8 流 Stream ( 上 )

流 ( Stream ) 是 Java 8 新增加的一个重磅级的功能。流是一个抽象层。有了流，我们就可以使用类似于 SQL 语句的声明方式来处理数据。

比如，下面的 SQL 语句

```SQL
SELECT max(grade), student_id, student_name FROM Students;
```

上面这条 SQL 会自动返回最高学习绩点的学生的信息，而全程，开发人员却不用直接面对任何计算和比较。

在流 （ Stream ) 出现之前，对于Java 中的集合框架的使用。开发人员不得不一次次的写一个循环，一次次的重复检查。当然了，这也什么，毕竟大家都是这样过来的。

更大的问题在于开发效率。面对当前的多核 CPU 计算机，面对并发编程。我们开发者常常会写出非常容易出错的并发执行的代码。

为了解决这些问题，Java 8 引入了流 ( Stream ) 这个概念，允许开发人员以声明的方式处理数据的同时，还能利用多核构架，而无需编写任何特殊的代码。

## 流是什么 ?

Java 中的 **流** ( Stream ) 表示来自 **源** ( source ) 的一系列对象，它支持统计、求和、求平均值等聚合操作。

流具有以下特征：

- **元素序列** : 流以顺序方式提供特定类型的一组元素。流只会按需获取/计算元素。但它从不存储元素。
- **源 ( Source )**：流可以将集合，数组或 I/O 资源作为输入源。
- **聚合操作**： 流支持聚合操作，如 `filter`、`map`、`limit`、`reduce`、`find`、`match` 等
- **管道 ( pipelining )**：大多数流操作都返回流本身，以便可以对其结果进行流水线操作。这些操作称为 **中间** 操作，它们的功能是获取输入，处理它们并将输出返回到目标。`collect()` 方法是一个终端操作，通常在流水线操作结束时出现，以标记流的结尾。
- **原子性迭代 ( Automatic iterations )** ： 与需要显式迭代的集合相比，流操作在内部对所提供的源元素进行迭代。

## 流的创建

Java 8 在推出流的同时，对集合框架也进行了一些比较大变更。主要是在 `Collection` 接口上提供了两种生成 Stream 的方法:

- `stream()` 方法，该方法以集合作为源，返回集合中的所有元素以在集合中出现的顺序组成的流。
- `parallelStream()` 方法，该方法以集合作为源，返回一个支持并发操作的流。

## 流支持的聚合操作

### forEach() 方法

Java 8 为 Stream 提供了一种新方法 `forEach()`，用于迭代流的每个元素。

下面的代码片段演示了如何使用 `forEach` 打印 `10` 个随机数。

```JAVA
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```

上面这个代码片段中，`Random` 对象的 `ints()` 方法会返回一个整数流。而 `limit()` 方法则限制了流中的元素个数。从某些方面说，可以理解为当源产生了 10 个随机数之后就关闭源。

### map() 方法

`map()` 方法会迭代流中的元素，并为每个元素应用一个方法，然后返回应用后的流。

例如下面的代码，使用 `map()` 方法把求出每个元素的平方，然后过滤掉重复的元素，最后在转换为列表集合

```JAVA
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);

//获取每个元素的平方
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```

`map( i -> i*i)` 操作求取流中每个元素的平方，并返回一个新的流。`distinct()` 方法则用于过滤流中的重复元素。

### filter() 方法

`filter()` 方法根据一个谓词来过滤元素。这个谓词是一个方法，以流中的每一个元素作为参数，如果返回 `true` 则会被过滤掉。

例如下面的代码，使用 `filter()` 方法过滤那些空字符串。

```JAVA
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");

int count = strings.stream().filter(string -> string.isEmpty()).count();
```