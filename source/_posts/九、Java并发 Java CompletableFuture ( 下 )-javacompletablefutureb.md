---
title: 九、Java并发 Java CompletableFuture ( 下 )
categories:
  - Java
tags:
  - Java并发
abbrlink: 47722b62
date: 2022-05-04 19:05:55
updated: 2022-05-18 18:46:39
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 九、Java并发 Java CompletableFuture ( 下 )

上一章节中我们讲解了 CompletableFuture 的一些基本用法，比如如何使用和如何处理异步计算结果。本章节我们继续，主要讲解如何使用 CompletableFuture 来组合异步计算的结果

## 组合 Futures

CompletableFuture API 最吸引人的部分，应该是能够在一系列链式计算步骤中组合 CompletableFuture 实例。这种链式的结果本身就是CompletableFuture，允许进一步链接和组合。

这种方法在函数式语言中无处不在，通常被称为 「一元 ( monadic ) 设计模式 」。

CompletableFuture 提供了方法 `thenCompose()` 用于按顺序链接两个 Futures。该方法的参数是一个能够返回 CompletableFuture 实例的函数或表达式。而该函数或表达式的参数则是先前计算步骤的结果，这允许我们在下一个 CompletableFuture 的 lambda 中使用这个值。

例如下面这个示例

```java
CompletableFuture<String> completableFuture 
  = CompletableFuture.supplyAsync(() -> "简单")
    .thenCompose(s -> CompletableFuture.supplyAsync(() -> s + "教程"));

assertEquals("搜云库技术团队", completableFuture.get());
```

`thenCompose()` 方法与 `thenApply()` 一起实现了一元设计模式的基本构建块，它们与Java 8 中提供的 Stream 和 Optional 类的 map 和flatMap 方法密切相关。

两个方法都接收一个函数并将其应用于计算结果，但 `thenCompose()` （ `flatMap()` ）方法接收一个函数，该函数返回相同类型的另一个对象，这样，就允许将这些类的实例组合为构建块。

如果要执行两个独立的 Futures 并对其结果执行某些操作，可以使用 Future 的 `thenCombine()` 并传递能够接收两个参数的函数或表达式来处理这两个结果。

```java
CompletableFuture<String> completableFuture 
  = CompletableFuture.supplyAsync(() -> "Hello")
    .thenCombine(CompletableFuture.supplyAsync(
      () -> " World"), (s1, s2) -> s1 + s2));

assertEquals("Hello World", completableFuture.get());
```

更简单的情况是，当你想要使用两个 Futures 的结果时，但有不想把任何结果值传递给 Future 链，则可以使用 `thenAcceptBoth()` 方法，如下所示

```java
CompletableFuture future = CompletableFuture.supplyAsync(() -> "Hello")
  .thenAcceptBoth(CompletableFuture.supplyAsync(() -> " World"),
    (s1, s2) -> System.out.println(s1 + s2));
```

## 并行执行多个 Future

当我们需要并行执行多个 Futures 时，通常是希望等待所有 Futures 执行完成然后处理它们的组合结果。

`CompletableFuture.allOf()` 静态方法允许等待作为 var-arg 提供的所有 Future 的完成。

```java
CompletableFuture<String> future1  
  = CompletableFuture.supplyAsync(() -> "Hello");
CompletableFuture<String> future2  
  = CompletableFuture.supplyAsync(() -> "Beautiful");
CompletableFuture<String> future3  
  = CompletableFuture.supplyAsync(() -> "World");

CompletableFuture<Void> combinedFuture 
  = CompletableFuture.allOf(future1, future2, future3);

// ...

combinedFuture.get();

assertTrue(future1.isDone());
assertTrue(future2.isDone());
assertTrue(future3.isDone());
```

> var-arg 的意思是：参数数量不定的意思。也就是可以传递任意相同类型的参数。

上面的示例中，你应该留意到了 `CompletableFuture.allOf()` 方法的返回类型是 `CompletableFuture <Void>`，这个方法的局限是它不会返回所有 Future 的综合结果。相反，你必须手动从 Futures 获取结果。幸运的是，`CompletableFuture.join()` 方法和 Java 8 Streams API 可以做到这一点

```java
String combined = Stream.of(future1, future2, future3)
  .map(CompletableFuture::join)
  .collect(Collectors.joining(" "));

assertEquals("Hello Beautiful World", combined);
```

`CompletableFuture.join()` 方法类似于 `get()` 方法，但是如果 Future 未正常完成，它会抛出未经检查的异常，这种机制，使得它可以作为 `Stream.map()` 的参数。

## 处理错误

对于异步计算步骤链中的错误处理，惯用的方法是调整 `throw/catch` 。

怎么个调整法呢 ？

CompletableFuture 类允许我们在特殊的 `handle()` 方法中处理它，而不是在语法块中捕获异常。

此 `handle()` 方法接收接收两个参数：计算结果（ 如果成功完成 ）和抛出异常（ 如果某些计算步骤未正常完成 ）。

```java
String name = null;

// ...

CompletableFuture<String> completableFuture  
  =  CompletableFuture.supplyAsync(() -> {
      if (name == null) {
          throw new RuntimeException("Computation error!");
      }
      return "Hello, " + name;
  })}).handle((s, t) -> s != null ? s : "Hello, Stranger!");

assertEquals("Hello, Stranger!", completableFuture.get());
```

上面这个示例中，我们使用 `handle()` 方法在问候语的异步计算完成时提供默认值，因为没有提供 `name` 。

作为替代方案，假设我们想要手动使用某个值完成 Future ，就像第一个示例中所示，但同时又需要有能力通过异常完成它。那么，可以使用 `completeExceptionally()` 方法。

```java
CompletableFuture<String> completableFuture = new CompletableFuture<>();

// ...

completableFuture.completeExceptionally(
  new RuntimeException("Calculation failed!"));

// ...

completableFuture.get(); // ExecutionException
```

上面这个示例的 `completableFuture.get()` 方法会抛出 `ExecutionException`，并使用`RuntimeException` 作为异常发生的原因。

在上面的例子中，我们也可以使用 `handle()` 方法异步处理异常，但使用 `get()` 方法是更典型的同步异常处理机制。

## 异步方法

CompletableFuture 类中的大多数流式 API 方法都又两个带有 `Async` 后缀的变体。这些变体方法通常用于在另一个线程中运行相应的执行步骤。

- 没有 `Async` 后缀的方法使用当前调用线程运行下一个执行阶段。
- 不带 Executor 参数的 Async 后缀方法使用 `ForkJoinPool.commonPool()` 方法访问 Executor 的公共 fork/join 线程池实现运行一个步骤。
- 带有 Executor 参数的 Async 后缀方法使用传递的 Executor 运行一个步骤。

下面这个范例中，我们使用了 Function 实例处理计算结果。和之前范例的唯一可见的区别就是 `thenApplyAsync()` 方法。但在幕后，函数的应用程序被包装到 ForkJoinTask 实例中（ 有关 fork/join 框架的更多信息，请阅读我们的 一文秒懂 Java Fork/Join ），这样可以进一步并行化我们的计算并更有效地使用系统资源。

```java
CompletableFuture<String> completableFuture  
  = CompletableFuture.supplyAsync(() -> "Hello");

CompletableFuture<String> future = completableFuture
  .thenApplyAsync(s -> s + " World");

assertEquals("Hello World", future.get());
```

## Java 9 CompletableFuture 新增的 API

Java 9 提供了一下变更进一步强化了 CompletableFuture：

- 添加了新的工厂方法
- 支持延时和超时
- 改进了对子类化的支持

Java 9 同时也引入了新的 CompletableFuture 实例 API

- Executor defaultExecutor()
- CompletableFuture newIncompleteFuture()
- CompletableFuture copy()CompletionStage minimalCompletionStage()
- CompletableFuture completeAsync(Supplier<? extends T> supplier, Executor executor)C
- ompletableFuture completeAsync(Supplier<? extends T> supplier)
- CompletableFuture orTimeout(long timeout, TimeUnit unit)
- CompletableFuture completeOnTimeout(T value, long timeout, TimeUnit unit)

还添加了一些静态的使用方法

- Executor delayedExecutor(long delay, TimeUnit unit, Executor executor)
- Executor delayedExecutor(long delay, TimeUnit unit)
- CompletionStage completedStage(U value)
- CompletionStage failedStage(Throwable ex)
- CompletableFuture failedFuture(Throwable ex)

最后，为了解决超时问题，Java 9 引入了另外两个新功能

- `orTimeout()`
- `completeOnTimeout()`