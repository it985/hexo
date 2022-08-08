---
title: 四、Java并发 Java 线程池之 ThreadPoolExecutor
categories:
  - Java
tags:
  - Java并发
abbrlink: f6cd6d86
date: 2022-05-04 18:49:44
updated: 2022-05-18 18:46:17
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 四、Java并发 Java 线程池之 ThreadPoolExecutor

因为上一章节篇幅有限，所以我决定把 **一文秒懂 Java 线程池** 拆分为三篇文章单独介绍。本章节，我们就来看看 `ThreadPoolExecutor` 。

## ThreadPoolExecutor

`ThreadPoolExecutor` 是一个可被继承 ( extends ) 的线程池实现，包含了用于微调的许多参数和钩子。

我们并不会讨论 ThreadPoolExecutor 类中的所有的参数和钩子，只会讨论几个主要的配置参数：

1.  `corePoolSize``
2. ``maximumPoolSize`
3.  `keepAliveTime`

ThreadPoolExecutor 创建的线程池由固定数量的核心线程组成，这些线程在 ThreadPoolExecutor 生命周期内始终存在，除此之外还有一些额外的线程可能会被创建，并会在不需要时主动销毁。`corePoolSize` 参数用于指定在线程池中实例化并保留的核心线程数。如果所有核心线程都忙，并且提交了更多任务，则允许线程池增长到 `maximumPoolSize` 。

`keepAliveTime` 参数是额外的线程（ 即，实例化超过 corePoolSize 的线程 ）在空闲状态下的存活时间。

这三个参数涵盖了广泛的使用场景，但最典型的配置是在 Executors 静态方法中预定义的。

### Executors.newFixedThreadPool()

例如，`Executors.newFixedThreadPool()` 静态方法创建了一个 `ThreadPoolExecutor` ，它的参数 `corePoolSize` 和 `maximumPoolSize` 都是相等的，且参数 `keepAliveTime` 始终为 `0` ，也就意味着此线程池中的线程数始终相同。

```java
ThreadPoolExecutor executor = 
  (ThreadPoolExecutor) Executors.newFixedThreadPool(2);
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});

assertEquals(2, executor.getPoolSize());
assertEquals(1, executor.getQueue().size());
```

上面这个示例中，我们实例化了一个固定线程数为 `2` 的 `ThreadPoolExecutor`。这意味着如果同时运行的任务的数量始终小于或等于 2 ，那么这些任务会立即执行。否则，其中一些任务可能会被放入队列中等待轮到它们。

上面这个示例中，我们创建了三个 `Callable` 任务，通过睡眠模拟 1000 毫秒的繁重工作。前两个任务将立即执行，第三个任务必须在队列中等待。我们可以通过在提交任务后立即调用 `getPoolSize()` 和 `getQueue().size()` 来方法来验证。

### Executors.newCachedThreadPool()

`Executors` 还提供了 `Executors.newCachedThreadPool()` 静态方法创建另一个预配置的 `ThreadPoolExecutor`。该方法创建的线程池没有任何核心线程，因为它将 `corePoolSize` 属性设置为 `0`，但同时有可以创建最大数量的额外线程，因为它将 `maximumPoolSize` 设置为 `Integer.MAX_VALUE` ，且将 `keepAliveTime` 的值设置为 `60` 秒。

这些参数值意味着缓存的线程池可以无限制地增长以容纳任何数量的已提交任务。但是，当不再需要线程时，它们将在 60秒不活动后被销毁。这种线程池的使用场景一般是你的应用程序中有很多短期任务。

```java
ThreadPoolExecutor executor = 
  (ThreadPoolExecutor) Executors.newCachedThreadPool();
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});
executor.submit(() -> {
    Thread.sleep(1000);
    return null;
});

assertEquals(3, executor.getPoolSize());
assertEquals(0, executor.getQueue().size());
```

上面这个示例中的队列大小始终为 `0` ，因为在内部使用了 `SynchronousQueue` 的实例。在 `SynchronousQueue` 中，插入和删除操作总是成对出现且同时发生。因此队列实际上从不包含任何内容。

### Executors.newSingleThreadExecutor()

`Executors.newSingleThreadExecutor()` 静态方法则创建另一种典型的只包含单个线程的 `ThreadPoolExecutor` 实例。

这种单线程执行程序是创建事件循环的理想选择。

在这个单线程 `ThreadPoolExecutor` 实例中，属性 `corePoolSize` 和属性 `maximumPoolSize` 的值都为 `1`，而属性 `keepAliveTime` 的值为 `0` 。

在单线程 `ThreadPoolExecutor` 实例中，所有的任务都按顺序执行。因此，下面的示例中，任务完成后标志的值是 `2`。

```java
AtomicInteger counter = new AtomicInteger();

ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(() -> {
    counter.set(1);
});
executor.submit(() -> {
    counter.compareAndSet(1, 2);
});
```

此外，单线程 `ThreadPoolExecutor` 实例使用了不可变包装器进行修饰，因此在创建后无法重新配置。当然了，这也是我们无法将该示例强制转换为 `ThreadPoolExecutor` 的原因。