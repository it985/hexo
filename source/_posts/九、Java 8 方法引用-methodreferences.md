---
title: 九、Java 8 方法引用
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 4d9ed831
date: 2022-05-05 18:58:11
updated: 2022-05-18 18:48:12
---

# 九、Java 8 方法引用

Java 8 中新增加了 **方法引用** 这个概念。 但，什么是方法引用呢 ？

我们先来看一个例子，下面这个范例演示了如何遍历字符串列表并进行一些操作

#### LambdaTester.java

```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class LambdaTester {

    public static void main(String[] args)
    {
        LambdaTester tester = new LambdaTester();
        tester.run();
    }

    public void run()
    {
        List<String> list = Arrays.asList("Ram","Shyam","Kabir");

        // 输出
        for(String st: list){
            System.out.println(st);
        }

        // 转换为大写
        for(String st: list){
            upperAndPrint(st);
        }
    }

    public static void upperAndPrint(String s)
    {
        System.out.println(s.toUpperCase());
    }
}
```

运行结果如下

```
Ram
Shyam
Kabir
RAM
SHYAM
KABIR
```

有简洁代码倾向的我们，看到那重复的 `for(String st: list)` 就会想如何能够直接把代码改的更简洁一些。

第一个想到的，肯定是使用 Java 8 新增的 lambda 表达式和 `forEach` 改造下

#### LambdaTester.java

```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class LambdaTester {

    public static void main(String[] args)
    {
        LambdaTester tester = new LambdaTester();
        tester.run();
    }

    public void run()
    {
        List<String> list = Arrays.asList("Ram","Shyam","Kabir");

        // 输出
        list.forEach(item -> System.out.println(item));


        // 转换为大写
        list.forEach(item -> upperAndPrint(item));
    }

    public static void upperAndPrint(String s)
    {
        System.out.println(s.toUpperCase());
    }
}
```

哇，使用 Lambda 表达式真的是简洁了不少，但是，还能更简单一点吗？

比如 `item -> System.out.println(item)` 这个表达式，其实就是调用 `System.out.println()` 方法，并把 `forEach` 迭代列表生成的唯一参数 `item` 传给它而已

比如 `item -> upperAndPrint(item)` 这个表达式，也是一样啊，就是把 `forEach` 生成的唯一参数 `item` 传给给 `upperAndPrint()` 方法而已。

既然这样，我们为什么不能直接传递方法名给 `forEach` 呢？

对吧，所以我们改改，改成如下这种方式

```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class LambdaTester {

    public static void main(String[] args)
    {
        LambdaTester tester = new LambdaTester();
        tester.run();
    }

    public void run()
    {
        List<String> list = Arrays.asList("Ram","Shyam","Kabir");

        // 输出
        list.forEach(System.out.println);

        // 转换为大写
        list.forEach(upperAndPrint);
    }

    public static void upperAndPrint(String s)
    {
        System.out.println(s.toUpperCase());
    }
}
```

运行结果如下

```
LambdaTester.java:18: 错误: 找不到符号
        list.forEach(System.out.println);
                               ^
  符号:   变量 println
  位置: 类型为PrintStream的变量 out
LambdaTester.java:21: 错误: 找不到符号
        list.forEach(upperAndPrint);
                     ^
  符号:   变量 upperAndPrint
  位置: 类 LambdaTester
2 个错误
```

这在所有的 Java 版本中都会报错，但是自 Java 8 开始，还真是可以直接传递方法名的，只是我们使用的方式不对而已。

加入我们改成下面这样，就会正确了

```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class LambdaTester {

    public static void main(String[] args)
    {
        LambdaTester tester = new LambdaTester();
        tester.run();
    }

    public void run()
    {
        List<String> list = Arrays.asList("Ram","Shyam","Kabir");

        // 输出
        list.forEach(System.out::println);

        // 转换为大写
        list.forEach(LambdaTester::upperAndPrint);
    }

    public static void upperAndPrint(String s)
    {
        System.out.println(s.toUpperCase());
    }
}
```

运行结果如下

```
Ram
Shyam
Kabir
RAM
SHYAM
KABIR
```

哈哈哈，正确了。

在这个正确的方法中，有两个重点：

1、 方法名和类名之间不是使用点号 ( `.` ) 而是使用两个冒号 `::` 。
2、 提供的方法名必须包含类名，如果没有引入该类名，则需要使用全限定类名，也就是需要添加包名作为前缀。

这种使用方法名做参数的做法，在 Java 8 中称之为 「 方法引用 」

## Java 8 方法引用

**方法引用** 是 Java 8 新增加的功能。方法引用有点类似于 C / C++ 中的 **函数指针** ，通过方法名称指向方法。

Java 8 中的方法引用通过 `::` 符号引用方法，而且支持一下类型的方法引用

1、 静态方法
2、 实例方法
3、 使用 `new` 运算符的构造函数。例如 `TreeSet::new`

## 范例

我们重写一下上面的范例，演示下如何引用静态方法和实例方法

```JAVA
import java.util.Arrays;
import java.util.List;
import java.util.function.Function;

public class LambdaTester {

    public static void main(String[] args)
    {
        LambdaTester tester = new LambdaTester();
        tester.run();
    }

    public void run()
    {
        List<String> list = Arrays.asList("Ram","Shyam","Kabir");

        // 输出
        list.forEach(System.out::println);

        // 转换为大写
        list.forEach(LambdaTester::upperAndPrint);

        // 转换为小写并输出
        list.forEach(this::lowerAndPrint);
    }

    public void lowerAndPrint(String s)
    {
        System.out.println(s.toLowerCase());
    }

    public static void upperAndPrint(String s)
    {
        System.out.println(s.toUpperCase());
    }
}
```

运行结果如下

```
Ram
Shyam
Kabir
RAM
SHYAM
KABIR
ram
shyam
kabir
```