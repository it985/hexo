---
title: 五、Java并发 ScheduledThreadPoolExecutor
categories:
  - Java
tags:
  - Java并发
abbrlink: 5975178e
date: 2022-05-04 18:51:38
updated: 2022-05-18 18:46:22
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 五、Java并发 ScheduledThreadPoolExecutor

`ScheduledThreadPoolExecutor` 扩展自 一文秒懂 Java 线程池之 ThreadPoolExecutor 讲解的 `了ThreadPoolExecutor` 类，并且添加了其它方法实现了 `ScheduledExecutorService` 接口。

- `schedule()` 方法允许在指定的延迟后执行一次任务
- `scheduleAtFixedRate()` 方法允许在指定的初始延迟后执行任务，然后以一定的周期重复执行，其中 `period` 参数用于指定两个任务的开始时间之间的间隔时间，因此任务执行的频率是固定的。
- `scheduleWithFixedDelay()` 方法类似于 `scheduleAtFixedRate()` ，它也重复执行给定的任务，但`period` 参数用于指定前一个任务的结束和下一个任务的开始之间的间隔时间。也就是指定下一个任务延时多久后才执行。执行频率可能会有所不同，具体取决于执行任何给定任务所需的时间。

静态方法 `Executors.newScheduledThreadPool()` 方法用于创建包含了指定 `corePoolSize`，无上限 `maximumPoolSize` 和 `0` 存活时间 `keepAliveTime` 的 `ScheduledThreadPoolExecutor` 实例。

例如下面的示例创建了一个包含了 5 个核心线程的 `ScheduledThreadPoolExecutor` 实例，且每隔 500 毫秒运行一个输出 `Hello World` 的任务

```java
ScheduledExecutorService executor = Executors.newScheduledThreadPool(5);
executor.schedule(() -> {
    System.out.println("Hello World");
}, 500, TimeUnit.MILLISECONDS);
```

## 范例 2

下面的代码则演示了如何在 500 毫秒延迟后执行任务，然后每 100 毫秒重复执行一次。

```java
CountDownLatch lock = new CountDownLatch(3);

ScheduledExecutorService executor = Executors.newScheduledThreadPool(5);
ScheduledFuture<?> future = executor.scheduleAtFixedRate(() -> {
    System.out.println("Hello World");
    lock.countDown();
}, 500, 100, TimeUnit.MILLISECONDS);

lock.await(1000, TimeUnit.MILLISECONDS);
future.cancel(true);
```