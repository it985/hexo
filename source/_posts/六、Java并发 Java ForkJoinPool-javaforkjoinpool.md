---
title: 六、Java并发 Java ForkJoinPool
categories:
  - Java
tags:
  - Java并发
abbrlink: b57a6e2b
date: 2022-05-04 18:53:48
updated: 2022-05-18 18:46:26
keywords:
top_img: img/hzw.webp
cover: img/hzw.webp
---

# 六、Java并发 Java ForkJoinPool

ForkJoinPool 是Java 7 中引入的 fork/join 框架的核心之一。它解决了一个常见的问题： 如何在递归中生成多个任务。因为，即使是使用一个简单的 `ThreadPoolExecutor` ，也会在不断的递归中快速耗尽线程。因为每个任务或子任务都需要自己的线程来运行。

在 fork/join 框架中，任何任务都可以生成 ( fork ) 多个子任务并使用 `join()` 方法等待它们的完成。fork/join 框架的好处是它不会为每个任务或子任务创建新线程，而是实现了 **工作窃取** ( Work Stealing ) 算法。关于 fork/join 框架的详细信息，你可以访问我们的 一文秒懂 Java Fork/Join。

接下来，我们看一个使用 `ForkJoinPool` 遍历节点树并计算所有叶值之和的简单示例。在这个示例中，树是一个由节点，int 值和一组子节点组成。

```java
static class TreeNode {

    int value;

    Set<TreeNode> children;

    TreeNode(int value, TreeNode... children) {
        this.value = value;
        this.children = Sets.newHashSet(children);
    }
}
```

创建了树 `TreeNode` 之后，如果我们想要并行地对树中的所有值求和，我们需要实现一个 `RecursiveTask<Integer>` 接口。每个任务都接收自己的节点，并将其值添加到其子节点的值之和上。

要计算子节点值的总和，任务实现执行以下操作

1、 将子节点集合转换为流 ( stream )
2、 映射前面操作中创建的流，为每个元素创建一个新的 CountingTask
3、 通过 `fork` 执行每个子任务
4、 通过在每个 `fork` 任务上调用 `join()` 方法来收集结果
5、 使用 ·Collectors.summingInt` 收集器对结果求和

```java
public static class CountingTask extends RecursiveTask<Integer> {

    private final TreeNode node;

    public CountingTask(TreeNode node) {
        this.node = node;
    }

    @Override
    protected Integer compute() {
        return node.value + node.children.stream()
          .map(childNode -> new CountingTask(childNode).fork())
          .collect(Collectors.summingInt(ForkJoinTask::join));
    }
}
```

在树上运行计算的代码非常简单：

```java
TreeNode tree = new TreeNode(5,
  new TreeNode(3), new TreeNode(2,
    new TreeNode(2), new TreeNode(8)));

ForkJoinPool forkJoinPool = ForkJoinPool.commonPool();
int sum = forkJoinPool.invoke(new CountingTask(tree));
```