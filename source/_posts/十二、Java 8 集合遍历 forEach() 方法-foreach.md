---
title: 十二、Java 8 集合遍历 forEach() 方法
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 8d0e4be4
date: 2022-05-05 19:02:46
updated: 2022-05-18 18:48:25
---

# 十二、Java 8 集合遍历 forEach() 方法

Java 8 为所有的集合添加了一个新的方法 `forEach()` ，该方法以只读形式遍历集合所有的元素并为每一个元素执行一个动作。

我们先来看一个范例

#### ForEachTester.java

```JAVA
import java.util.Arrays;

public class ForEachTester
{
    public static void main(String[] args)
    {
        Arrays.asList("你好", "A", "B ").forEach(System.out::println);
    }
}
```

上面这段代码中，我们使用了 `forEach` 方法遍历列表的每一个元素，并把元素传递给 `System.out.println()` 方法打印输出到屏幕上。

对于这个功能，如果我们使用 `for ( : )` 语法，则会又有些小复杂

```JAVA
import java.util.Arrays;
import java.util.List;

public class ForEachTester
{
    public static void main(String[] args)
    {
        List<String> list = Arrays.asList("你好", "A", "B ");
        for( String it : list)
        {
            System.out.println(it);
        }
    }
}
```

## forEach() 方法

`forEach()` 方法是 Java 8 为所有集合新增的方法。该方法定义在 `java.lang.Iterable` 接口中。

`java.lang.Iterable` 接口是 Java 5 引入的，目的在于为实现该语句的对象提供 「 for-each 循环 」 语句。换句话说，所有实现了该接口的对象都可以使用 `for` 语句进行迭代。

当然了，我们今天的重点是 `forEach()` 这个方法。该方法的原型如下

```JAVA
default void forEach(Consumer<? super T> action)
```

从函数原型中可以看出，该方法是 `java.lang.Iterable` 接口的默认方法，所有子类可以不用实现，也没必要实现。

该方法对 `Iterable` 中的的每个元素执行给定的操作 ( `action` )，直到处理完所有元素或操作抛出异常为止。

除非实现类另有指定，否则操作会按迭代顺序执行（ 如果指定了迭代顺序 ）。且操作抛出的异常将转发给调用者。

该方法的的实现类似于

```JAVA
for (T t : this)
{
   action.accept(t);
}
```

## 范例一：forEach() 方法迭代列表

我们写一个范例使用 `forEach()` 方法和方法引用来实现一个列表的迭代

#### ForEachTester.java

```JAVA
import java.util.Arrays;
import java.util.List;

public class ForEachTester
{
    public static void main(String[] args)
    {
        List<String> list = Arrays.asList("你好", "A", "B");
        for( String it : list)
        {
            System.out.println(it);
        }
    }
}
```

运行结果如下

```
你好
A
B
```

## 范例二： forEach() 方法迭代一个哈希表

我们写一个范例使用 `forEach()` 方法和 Java 8 提供的 lambda 表达式来迭代一个哈希表

#### ForEachTester.java

```JAVA
import java.util.HashMap;
import java.util.Map;

public class ForEachTester
{
    public static void main(String[] args)
    {
        Map<String, Integer> items = new HashMap<>();
        items.put("A", 10);
        items.put("B", 20);
        items.put("C", 30);
        items.put("D", 40);
        items.put("E", 50);
        items.put("F", 60);

        items.forEach((k,v)->System.out.println("Item : " + k + " Count : " + v));

        items.forEach((k,v)->{
            System.out.println("Item : " + k + " Count : " + v);
            if("E".equals(k)){
                System.out.println("Hello E");
            }
        });
    }
}
```

运行结果如下

```
Item : A Count : 10
Item : B Count : 20
Item : C Count : 30
Item : D Count : 40
Item : E Count : 50
Item : F Count : 60
Item : A Count : 10
Item : B Count : 20
Item : C Count : 30
Item : D Count : 40
Item : E Count : 50
Hello E
Item : F Count : 60
```

## 范例三：forEach() 方法迭代一个流

我们写一个范例使用 `forEach()` 方法迭代一个流，输出流中的所有元素

#### ForEachTester.java

```JAVA
import java.util.Arrays;

public class ForEachTester
{
    public static void main(String[] args)
    {
        Arrays.asList("你好","A","B ")
            .stream()
            .filter(s->s.contains("C"))
            .forEach(System.out::println);
    }
}
```

运行结果如下

```
A
B
```