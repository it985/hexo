---
title: 七、Java并发 Java Google Guava 实现
categories:
  - Java
tags:
  - Java并发
abbrlink: cb5d7b3f
date: 2022-05-04 18:56:19
updated: 2022-05-18 18:46:30
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 七、Java并发 Java Google Guava 实现

[Guava](https://github.com/google/guava) 是托管在 [Github.com](https://github.com/) 上的流行的 Google 开源的 Java 线程池库。

Guava 包含了许多有用的并发类，同时还包含了几个方便的 `ExecutorService` 实现，但这些实现类都无法通过直接实例化或子类化来创建实例。取而代之的是提供了 MoreExecutors 助手类来创建它们的实例。

## 给 Maven 添加 Guava 依赖

为了将 Google Guava 库包含进当前的项目中，需要将下面的依赖项添加到 Maven pom 文件中。

```xml
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>26.0</version>
</dependency>
```

你可以在 [Maven 中央仓库](https://search.maven.org/classic/#search|gav|1|g%3A"com.google.guava" AND a%3A"guava") 中找到最新版本的 Guava 库

## 直接执行者和直接执行者服务

有时候，我们希望在当前线程或线程池中执行任务，具体在哪里取决于某些条件。这种情况下，你应该会更喜欢使用单个 Executor 接口，且只需切换实现即可。

虽然将当前线程中的任务的 Executor 或 ExecutorService 的提取出来单独实现并不困难，但它仍然需要编写一些样板代码。

值得庆幸的是，Guava 为我们提供了预定义的实例。

下面的范例演示了如何在同一个线程中执行任务。简单起见，提交的任务会将当前线程休眠 500 毫秒并阻塞当前线程，并在执行的调用完成后让结果立即可用

```java
 Executor executor = MoreExecutors.directExecutor();

AtomicBoolean executed = new AtomicBoolean();

executor.execute(() -> {
    try {
        Thread.sleep(500);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    executed.set(true);
});

assertTrue(executed.get());
```

`directExecutor()` 方法返回的实例实际上是一个静态单例，因此使用此方法根本不会在对象创建上带来任何开销。

你应该更喜欢使用此方法来访问 `MoreExecutors.newDirectExecutorService()`，因为该 API 会在每次调用时创建完整的执行程序服务实现。

## 退出 Executor 服务

另一个常见问题是： **在线程池仍在运行其任务时关闭虚拟机**。即使采用了取消机制，也无法保证任务执行良好，并在执行程序服务 （ Executor ）关闭时停止工作。这可能会导致 JVM 在任务继续工作时无限期挂起。

为了解决这个问题，Guava 引入了一系列已经实例化好的执行器 （ Executor ） 服务。它们是守护线程模式，但会与 JVM 一起终止。

这些执行器服务还提供了 `Runtime.getRuntime().addShutdownHook()` 方法用于添加一个关闭钩子，用于设置 VM 在放弃挂起的任务之前等待一段预配置的超时时间。

下面的示例中，我们提交了一个无限循环的任务，我们使用了包含 100 毫秒超时时间的已经存在的执行程序服务来运行任务，并在超过配置的超时时间之后终止 VM 。如果没有 exitingExecutorService ，此任务将导致 VM 无限期挂起。

```java
ThreadPoolExecutor executor = 
  (ThreadPoolExecutor) Executors.newFixedThreadPool(5);
ExecutorService executorService = 
  MoreExecutors.getExitingExecutorService(executor, 
    100, TimeUnit.MILLISECONDS);

executorService.submit(() -> {
    while (true) {
    }
});
```

## 监听装饰器

监听装饰器允许我们封装 ExecutorService 并在提交任务时返回 ListenableFuture 实例而不是简简单单的 Future 实例。

ListenableFuture 接口扩展自 Future 接口，并添加了一个新方法 `addListener()`，该方法用于添加在将来完成时调用的侦听器。

一般情况下，我们很少直接使用 `ListenableFuture.addListener()` 方法，而是使用 Futures 类提供的许多辅助方法。例如，通过`Futures.allAsList()` 方法，我们可以在单个 ListenableFuture 中组合多个 ListenableFuture 实例，并会在这些实例在成功完成后将所有的 futures 合并并返回结果。

```java
ExecutorService executorService = Executors.newCachedThreadPool();
ListeningExecutorService listeningExecutorService = 
  MoreExecutors.listeningDecorator(executorService);

ListenableFuture<String> future1 = 
  listeningExecutorService.submit(() -> "java");
ListenableFuture<String> future2 = 
  listeningExecutorService.submit(() -> "hello");

String greeting = Futures.allAsList(future1, future2).get()
  .stream()
  .collect(Collectors.joining(""));
assertEquals("云遇未晚", greeting);
```