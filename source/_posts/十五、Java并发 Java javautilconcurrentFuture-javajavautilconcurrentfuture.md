---
title: 十五、Java并发 Java java.util.concurrent.Future
categories:
  - Java
tags:
  - Java并发
abbrlink: 50778af3
date: 2022-05-04 19:17:30
updated: 2022-05-18 18:47:14
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 十五、Java并发 Java java.util.concurrent.Future

写了几篇 Java 一文秒懂 XXX 系列的文章后，对 Java 并发编程的设计思想真的是竖然起敬。

Java 在并发方面引入了 「 将来 」( Future ) 这个概念。把所有不在主线程执行的代码都附加了将来这个灵魂。主线程只负责其它并发线程的创建、启动、监视和处理并发线程完成任务或发生异常时的回调。其它情况，则交给并发线程自己去处理。而双方之间的沟通，就是通过一个个被称之为 「 将来 」 的类出处理。

`Future` 定义在 `java.util.concurrent` 包中，这是一个接口，自 Java 1.5 以来一直存在的接口，用于处理异步调用和处理并发编程。

## 创建 `Future`

简单地说，`Future` 类表示异步计算的未来结果 – 在处理完成后最终将出现在 Future 中的结果。

是不是又很难理解，文字越少，内容越多。上面这句话的意思，就是主线程会创建一个 Future 接口的对象，然后启动并发线程，并告诉并发线程，一旦你执行完毕，就把结果存储在这个 Future 对象里。

因此，理解 Future 的第一步，就是要知道如何创建和返回 Future 实例。

一般情况下，我们会把长时间运行的逻辑放在异步线程中进行处理，这是使用 Future 接口最理想的场景。主线程只要简单的将异步任务封装在 Future 里，然后开始等待 Future 的完成，在这段等待的时间内，可以处理一些其它逻辑，一旦 Future 执行完毕，就可以从中获取执行的结果并进一步处理。

针对上面这种表述，我们来看看具体哪些场景可以使用 Future :

- 计算密集型（ 数学和科学计算 ）
- 操纵大数据结构（ 大数据 ）
- 远程方法调用（下载文件，HTML 爬取，Web 服务）

### 实现了 Future 的 FutureTask

我们先来看一段代码:

```java
public class SquareCalculator {    

    private ExecutorService executor 
      = Executors.newSingleThreadExecutor();

    public Future<Integer> calculate(Integer input) {        
        return executor.submit(() -> {
            Thread.sleep(1000);
            return input * input;
        });
    }
}
```

如果你认真读过前几个章节，想必对这段代码不陌生了。

在上面这段代码中，我们创建了一个简单的类用于计算一个整型 ( Integer ) 的平方。当然了，计算平方这个任务肯定不能划到 「 长时间运行 」 这个类别里，所以我们在它之前又添加了 `Thread.sleep(1000)`。

> 不要小看 1s。这已经是相当长的任务了。

在上面这段代码中，实际执行的计算是作为 Lambda 表达式参数传递给 `call()` 方法。当然了，这个实际执行的代码，除了 `Thread.sleep()` 之外好像也没有什么特别之处。

好了，现在，我们应该将注意力转移到 Callable 和 ExecutorService 的使用，因为它们才是最有趣的。

`Callable` 是一个接口，用于表示一个任务，这个任务可以返回值。`Callable` 接口只有一个方法 `call()`。上面的示例中。那个 Lambda 其实就是一个 Callable 实例。

> 啥？ 不会看不懂吧？ 好吧，我找个时间好好写一些 Java Lambda 方面的文章。

Callable 实例创建完成后并不会立即执行，我们仍然需要将它传递给一个 「 执行器 」( Executor , 执行程序 ) ，这个执行器将负责在新线程中启动该任务并返回一个包含了值的 Future 对象。

这个执行器，是 `Executor` 的实例，通常，它是一个 ExecutorService 类的实例。

Java 其实提供了很多方法创建 `ExecutorService` 的实例，但最常用的，也是最推荐的做法是使用 Executors 的静态工厂方法。上面的示例中，我们就使用了 `Executors.newSingleThreadExecutor()` 方法创建了一个能够处理单个线程的 ExecutorService。

一旦我们有了一个 ExecutorService 对象，我们只需要调用它的 `submit()` 并传递我们的 Callable 作为参数即可。 `submit()` 会启动任务并返回一个 FutureTask 对象。

FutureTask 是一个类，实现了 Future 接口， 在 `java.util.concurrent` 包中定义。

## 消费( 使用 ) Future

用了相当长的篇幅，我们终于讲完了如何创建一个 Future 实例，接下来，我们将进入如何消费(使用) 刚刚创建的 Future 实例。

### 使用 `isDone()` 和 `get()` 方法来获取结果

现在，是时候调用 `calculate()` 方法获取返回的 Future 实例了，通过 Future 实例，我们就能进一步获取计算的整型结果。

要从 Future 实例中获取结果，我们需要用到两个方法：`isDone()` 和 `get()`。

1、 `Future.isDone()` 方法用于获取我们的执行器是否已完成任务处理。如果任务完成，则返回 `true`，否则返回 `false`。
2、 从计算中返回实际结果的方法是 `Future.get()`。但要注意的是，`Future.get()` 方法是一个阻塞方法。如果任务还没执行完毕，那么会一直阻塞直到直到任务完成，

为了防止调用 `Future.get()` 方法阻塞当前线程，推荐的做法是先调用 `Future.isDone()` 判断任务是否完成，然后再调用 `Future.get()` 从完成的任务中获取任务执行的结果。

因为 `Future.isDone()` 和 `Future.get()` 的存在，我们就可以在等待任务完成时运行其它一些代码，就像下面示例中所演示的那样

```java
Future<Integer> future = new SquareCalculator().calculate(10);

while(!future.isDone()) {
    System.out.println("Calculating...");
    Thread.sleep(300);
}

Integer result = future.get();
```

上面这段代码，我们在等待计算任务完成的同时执行了一条输出语句，用于提醒用户当前程序还是在运行的，并没有僵死。

我们使用了一个 `while` 循环，使用 `future.isDone()` 来检查任务是否完成，一旦完成，就会立即终止循环，并调用 `future.get()` 方法获取计算的结果。

因为实现使用了 `isDone()` 判断任务是否完成，所以 `future.get()` 并不会发生阻塞，想法，简直就是立即返回。

使用 `isDone()` 和 `get()` 方法来获取结果，这应该是消费 Future 最常见的方式。

当然了，值得一提的是，`Future.get()` 方法有一个可以超时等待的重载版本，这个重载版本接收两个参数，一个是超时的时间，另一个是超时时间的单位。方法原型如下

```java
V get(long timeout, TimeUnit unit) throws InterruptedException,ExecutionException,TimeoutException
```

而使用方式如下

```java
Integer result = future.get(500, TimeUnit.MILLISECONDS);
```

`get(long, TimeUnit)` 和 `get()` 的不同之处，是前者在经过指定的超时时间后任务仍未返回，那么就会抛出一个 `TimeoutException` 异常，表示执行超时。

### 使用 `Future.cancel()` 方法取消 Future

假设我们已经触发了一项任务，但由于某种原因，我们不再关心结果了。我们可以使用 `Future.cancel(boolean)` 告诉执行器停止操作并中断其底层线程。该方法很简单，使用演示如下

```java
Future<Integer> future = new SquareCalculator().calculate(4);

boolean canceled = future.cancel(true);
```

上面这两行代码，我们的 Future 实例永远不会完成它的操作。实际上，如果我们尝试在调用了 `cancel()` 方法之后立即调用 `get()` 方法，将会获得一个 `CancellationException` 异常。

为了防止 `Future.get()` 抛出一个 `CancellationException` 异常，我们可以使用 `Future.isCancelled()` 检查 Future 是否已被取消。

#### 注意

1、 对 `cancel()` 的调用可能会失败。如果调用失败，那么它会返回 `false`。
2、 cancel() 方法接受一个布尔值作为参数，该参数用于控制执行此任务的线程是否应该被中断。

## 多线程 vs 线程池

上面的示例中，我们的 `ExecutorService` 实例是单线程的，因为它是使用 `Executors.newSingleThreadExecutor()` 方法获得的。

为了突出演示它是 「 单线程 」，我们改一下代码同时触发两个计算

```java
SquareCalculator squareCalculator = new SquareCalculator();

Future<Integer> future1 = squareCalculator.calculate(10);
Future<Integer> future2 = squareCalculator.calculate(100);

while (!(future1.isDone() && future2.isDone())) {
    System.out.println(
      String.format(
        "future1 is %s and future2 is %s", 
        future1.isDone() ? "done" : "not done", 
        future2.isDone() ? "done" : "not done"
      )
    );
    Thread.sleep(300);
}

Integer result1 = future1.get();
Integer result2 = future2.get();

System.out.println(result1 + " and " + result2);

squareCalculator.shutdown();
```

然后我们就会获得类似下面的输出

```sh
calculating square for: 10
future1 is not done and future2 is not done
future1 is not done and future2 is not done
future1 is not done and future2 is not done
future1 is not done and future2 is not done

calculating square for: 100
future1 is done and future2 is not done
future1 is done and future2 is not done
future1 is done and future2 is not done
100 and 10000
```

很明显，整个过程并不是并行执行的。因为第二个任务仅在第一个任务完成后才开始，所以，整个过程大约需要 2 秒钟才能完成。

为了使我们的程序真正具有多线程，我们应该使用不同风格的 `ExecutorService` 。例如下面这段代码，我们使用工厂方法`Executors.newFixedThreadPool()` 创建一个固定大小的线程池，并观察输出的结果有何变化。

```java
public class SquareCalculator {

    private ExecutorService executor = Executors.newFixedThreadPool(2);

    //...
}
```

> 我比较懒，你把相应的代码替换下即可，省略号那段就不用替换了。

这段代码，对 SquareCalculator 类的做了一处简单的更改，使得我们的执行器拥有了 2 个同步线程。

如果我们再次运行完全相同的客户端代码，我们获得的输出可能如下

```sh
calculating square for: 10
calculating square for: 100
future1 is not done and future2 is not done
future1 is not done and future2 is not done
future1 is not done and future2 is not done
future1 is not done and future2 is not done
100 and 10000
```

现在看起来心情是否愉快多了，你应该留意到了， 2 个任务是如何同时开始和结束运行的，整个过程大约需要 1 秒钟就能完成。