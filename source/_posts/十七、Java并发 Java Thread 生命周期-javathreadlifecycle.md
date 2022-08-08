---
title: 十七、Java并发 Java Thread 生命周期
categories:
  - Java
tags:
  - Java并发
abbrlink: 9c8c1a2d
date: 2022-05-04 19:23:54
updated: 2022-05-18 18:47:22
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 十七、Java并发 Java Thread 生命周期

本文中，我想详细的讨论下 Java 中的核心概念 – 线程的生命周期。我会使用一张我自制的图片加上实用的代码片段，一步一步的详细剖析线程的各个状态和各个状态之间如何转换。

## Java 中的多线程

Java 语言中， 多线程是由 Thread 的核心概念驱动的。因为多线程中的每一个线程都相互独立，有着自己的生命周期和状态转换。

我们先来看一张草图，这图描述了 Java 线程的各种状态和转换过程。

![img_1.png](https://s3.bmp.ovh/imgs/2022/05/04/b5b70b4c71c1fcaa.png)

是不是很杂乱无章？ 看不懂没关系，我们接下来会详细介绍各个状态。

## Java 线程中的生命周期

Java 中，每一个线程都是 `java.lang.Thread` 类的实例。而且，Java 个线程生命周期中的各个状态都定义在 `Thread` 类的一个静态的 `State` 枚举中。

`State` 枚举定义了线程的所有潜在状态。总共有 6 个，分别对应者上图中的 6 个绿色背景的矩形和椭圆型。

- `NEW` : 新创建的，且未调用 `start()` 方法开始执行的线程。
- `RUNNABLE` : 已经在运行中的线程或正在等待资源分配的准备运行的线程。
- `BLOCKED` : 等待获取进入或重新进入同步块或方法的监视器锁的线程。
- `WAITING` : 等待其他一些线程执行特定操作，没有任何时间限制。
- `TIMED_WAITING`: 等待某个其他线程在指定时间段内执行特定操作
- `TERMINATED` : 线程完成了它的任务。

需要注意的是： **在任何给定的时间点，线程只能处于这些状态之一**。

- `NEW` 状态，应该很好理解，比如，车，厂家生产出来，只要还没被卖出过，那么它就是新的 ( NEW )
- `RUNNABLE` 只要线程不出于其它状态，它就是 RUNNABLE 状态。怎么理解呢？ 车买来了，只要它没坏没出什么毛病没借给别人，那么它就出于可开状态，不管是呆在家里吃灰还是已经在上路运行。
- `WAITING` : 无时间显示的等待其它线程完成任务时就处于这个状态，怎么理解呢？比如长假告诉公路大堵车，要等待别人前进了几个蜗牛步我们才能往前几个蜗牛步，有时候一等就是昏天暗地，可能长达几天，也可能，一辈子吧。
- `TIMED_WAITING` : 一直处于 `WAITING` 总不是办法，所以可能会设置一个超时时间，如果过了时间，就不等待了。同样的，如果可以后退，那么我们在堵车的时候可能会等待那么十几分钟，发现确实走不了，就等了呗。
- `TERMINATED` : 当一个线程结束了它的任务（可能完成了，也可能没完成）就会处于这个状态。如果拿车做比喻，那么当车彻底报废，已经再也不能上路了，就处于这个状态。

> 其实拿车作比喻感觉有点怪，我觉得拿追女朋友来做比喻比较恰当些。

## NEW 状态

**NEW**状态的线程（或已经创建的新线程）是已创建但尚未启动的线程。线程会一直保持这个 `NEW` 状态，直到在该线程上调用了 `start()` 方法启动它。

下面的代码，我们创建了一个 `NEW` 状态的线程

```java
Runnable runnable = new NewState();
Thread t = new Thread(runnable);
Log.info(t.getState());
```

由于我们没有启动线程，因此 `t.getState()` 方法将打印输出

```
NEW
```

## RUNNABLE 状态

当在一个 `NEW` 状态的线程上调用 `start()` 方法时，该线程的状态会从 `NEW` 转换为 `RUNNABLE`。处于该状态的线程要么是已经在运行中，那么是在处于正在等待系统的资源分配（准备运行）。

在多线程环境中，线程调度器 （ Thread-Scheduler，它是 JVM 的一部分）会为每个线程分配固定的时间。线程并不是一直都在执行的，调度器会把暂时空闲的线程的 CPU （ 还是在 RUNNABLE 状态 ）让出来，让其它需要的线程去运行。因此它会运行一段特定的时间，然后将控制权放弃给其他 RUNNABLE 线程。

> 注意： 这里的等待资源，不是等待其它线程，而是等待 CPU 排队。打个比方，新车上路。要等待的是有没有路，如果没有路，就开不了，这是本质的问题。

例如，让我们将 `t.start()` 方法添加到我们之前的代码中并尝试访问其当前状态

```java
Runnable runnable = new NewState();
Thread t = new Thread(runnable);
t.start();
Log.info(t.getState());
```

此代码最有可能返回输出

```
RUNNABLE
```

为什么说是最有可能呢？如果是一个空转线程，除了 CPU 不需要其它资源，那么很大概率就是 `RUNNABLE` ，但如果需要其它资源，可能会因为竞争资源而处于其它状态。还有一种情况，可能还没运行到 `t.getState()` ，线程任务就执行完毕了，那么也不会是 `RUNNABLE` 状态。

## BLOCKED 状态

当一个线程当前没有资格运行时，它处于 `BLOCKED` 状态。如果线程在尝试访问由某个其他线程锁定的代码段时，那它会因为需要等待获取监视器锁进入此状态。

我们使用一小段代码来重现下这个状态

```java
public class BlockedState {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new DemoThreadB());
        Thread t2 = new Thread(new DemoThreadB());

        t1.start();
        t2.start();

        Thread.sleep(1000);

        Log.info(t2.getState());
        System.exit(0);
    }
}

class DemoThreadB implements Runnable {
    @Override
    public void run() {
        commonResource();
    }

    public static synchronized void commonResource() {
        while(true) {
            // Infinite loop to mimic heavy processing
            // 't1' won't leave this method
            // when 't2' try to enters this
        }
    }
}
```

在上面这段代码中

- 我们创建了两个不同的线程– `t1` 和 `t2` 。
- `t1` 启动后就进入了同步的 `commonResource()`方法，同步方法意味着一次只能有一个线程可以访问它。尝试访问此方法的所有其他后续线程将被阻止进一步执行，直到当前线程完成处理。
- 当 t1 进入这个方法时，它保持了无限循环，这只是为了模仿繁重的处理，以便所有其他线程都无法进入此方法。
- 接着我们开启 `t2` ，它尝试输入已经被 `t1` 访问的 `commonResource()` 方法，这时，因为 `commonResource()` 被 t1 锁定，所以 t2 将保持在 `BLOCKED` 状态

在这个状态上，当我们使用 `t.getState()` 时将输出

```
BLOCKED
```

## WAITTING 状态

线程在等待某个其他线程执行特定操作时处于 `WAITING` 状态。根据 [Oracle 官方文档](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.State.html#WAITING)，任何线程都可以通过调用以下三种方法中的任何一种来进入此状态：

1、`object.wait()`

2、`thread.join()`

3、`LockSupport.park()`

请注意，我们没有为 `wait()` 和 `join()` 定义任何超时时间，因为下一节将介绍该方案。

我们以后会写一个单独的教程，详细讨论了 `wait()`、`notify()` 和 `notifyAll()` 的使用。

下面，我们写一段代码尝试重现这种状态

```java
public class WaitingState implements Runnable {
    public static Thread t1;

    public static void main(String[] args) {
        t1 = new Thread(new WaitingState());
        t1.start();
    }

    public void run() {
        Thread t2 = new Thread(new DemoThreadWS());
        t2.start();

        try {
            t2.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            Log.error("Thread interrupted", e);
        }
    }
}

class DemoThreadWS implements Runnable {
    public void run() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            Log.error("Thread interrupted", e);
        }

        Log.info(WaitingState.t1.getState());
    }
}
```

我们来讨论一下上面的代码做的事情

1、首先，我们创建并启动了 `t1`

2、其次，`t1` 创建了 `t2` 并启动它

3、当 `t2` 的处理继续时，我们调用 `t2.join()`，这使 `t1` 处于 `WAITING` 状态，直到 `t2` 完成执行

4、由于 t1 正在等待 t2 完成，我们从 t2 调用 `t1.getState()`

输出结果一般为

```
WAITING
```

请留意在哪里调用 `t1.getState()` 。

所以，`WAITING` 和 `BLOCKED` 两个状态的区别是什么？

- `BLOCKED` 是因为线程竞争不到资源而处于 BLOCKED 状态。这个是被动的。因为别无选择。
- `WAITING` 是因为线程主动等待别人完成而处于 WAITING 状态。这个是主动的。因为它可以不调用那三个方法，不用等待其它人完成。它可以选择挥一挥衣袖，不不带走一片云彩

## TIMED_WAITING 状态

线程在等待另一个线程在规定的时间内执行特定操作时处于 `TIMED_WAITING` 状态。根据 [Java Docs 文档](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.State.html#TIMED_WAITING)，有五种方法可以将线程置于TIMED_WAITING 状态：

- `thread.sleep(long millis)`
- ``wait(int timeout) or wait(int timeout, int nanos)`
- thread.join(long millis)
- LockSupport.parkNanos
- LockSupport.parkUntil`

下面，我们写一段代码尝试重现这种状态

```java
public class TimedWaitingState {
    public static void main(String[] args) throws InterruptedException {
        DemoThread obj1 = new DemoThread();
        Thread t1 = new Thread(obj1);
        t1.start();

        // The following sleep will give enough time for ThreadScheduler
        // to start processing of thread t1
        Thread.sleep(1000);
        Log.info(t1.getState());
    }
}

class DemoThread implements Runnable {
    @Override
    public void run() {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            Log.error("Thread interrupted", e);
        }
    }
}
```

整体代码和 `WAITING` 状态的差不多，我们创建并启动了一个线程 `t1`，并它进入睡眠状态，超时时间为 `5` 秒。

输出结果为

```
TIMED_WAITING
```

## TERMINATED 状态

这是一个 「 已死 」 线程的状态。当一个线程已经完成执行或异常终止时，它处于 `TERMINATED` 状态。我们写一段代码尝试重现这种状态

```java
public class TerminatedState implements Runnable {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new TerminatedState());
        t1.start();
        // The following sleep method will give enough time for 
        // thread t1 to complete
        Thread.sleep(1000);
        Log.info(t1.getState());
    }

    @Override
    public void run() {
        // No processing in this block
    }
}
```

上面这段代码中，我们启动线程 `t1` 时，下一个语句 `Thread.sleep(1000)` 为 t1 提供了足够的时间来完成。

因此，上面这个示例输出结果为

```sh
TERMINATED
```

## 后记

在本教程中，我们了解了 Java 中线程的生命周期。我们详细介绍了 `Thread.State` 枚举定义的所有七个状态，并使用一些示例来演示他们。

虽然代码片段几乎可以在每台机器上提供相同的输出，但在某些特殊情况下，我们可能会得到一些不同的输出，因为线程调度程序的确切行为无法确定。

所以，有任何问题，欢迎回帖咨询