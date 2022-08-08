---
title: 七、Java 8 Lambda 表达式 （ 中 ）- 外部参数
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 5fb1bf39
date: 2022-05-05 18:54:08
updated: 2022-05-18 18:48:02
---

# 七、Java 8 Lambda 表达式 （ 中 ）- 外部参数

在 Java 8 Lambda 表达式 （ 上 ）- 简介 章节中我们讲解了 Java 8 Lambda 表达式的一些基础知识。我们也了解 Java 8 Lambda 表达式的一些使用场景：

- Java Lambda 表达式主要用于定义函数接口的内联实现。而函数接口，就是只包含了一个方法的接口。在前一章节中，我们使用了各种类型的 lambda 表达式来定义 `MathOperation` 接口的 `operation` 方法。
- Java Lambda 表达式消除了对 **匿名类** 的需求，并为 Java 提供了非常简单但功能强大的函数编程功能。

## Java 8 Lambda 表达式作用域 ( scope )

因为 Java 8 的 lambda 表达式其实是函数接口的内联实现，也就是匿名内部类，因此，可以引用任何外部的变量或者常量。

但是，lambda 对这些外部的变量是有要求的： 它们必须使用 `final` 修饰符修饰。

如果一个变量允许被第二次赋值，则 Lambda 表达式会抛出编译错误。

> 注意： 其实这条规则并不是非常严格执行的，普通变量也是可以的，只要，只要不进行第二次赋值就可以。
>
> 注意： 刚刚测试了下，其实只要不是当前作用域声明的变量，可以随意第二次赋值，也不会报错

## 范例一

Java 8 lambda 表达式使用外部 `final` 变量

#### LambdaTester.java

```JAVA
public class LambdaTester
{

   final static String salutation = "你好，";

   public static void main(String args[])
   {
      GreetingService greetService1 = message -> 
      System.out.println(salutation + message);
      greetService1.sayMessage("A");
   }

   interface GreetingService {
      void sayMessage(String message);
   }
}
```

运行结果如下

```
你好，A
```

### 范例二

lambda 引用的普通的变量也是可以的，只要这个变量没有第二次被赋值，不管是任何地方。

#### LambdaTester.java

```JAVA
public class LambdaTester
{

   static String salutation = "你好，";

   public static void main(String args[])
   {
      GreetingService greetService1 = message -> 
      System.out.println(salutation + message);
      greetService1.sayMessage("B");
   }

   interface GreetingService {
      void sayMessage(String message);
   }
}
```

运行结果如下

```
你好，B
```

## 范例三

如果 lambda 表达式引用的是当前作用域下的普通的变量，而该变量又在某个地方第二次被赋值，则会抛出一个编译错误

#### LambdaTester.java

```JAVA
public class LambdaTester
{
   public static void main(String args[])
   {
      String salutation = "你好，";

      GreetingService greetService1 = message -> 
      System.out.println(salutation + message);
      greetService1.sayMessage("C");
      salutation = "Hello，";
   }

   interface GreetingService {
      void sayMessage(String message);
   }
}
```

运行结果如下

```
LambdaTester.java:8: 错误: 从lambda 表达式引用的本地变量必须是最终变量或实际上的最终变量
      System.out.println(salutation + message);
                         ^
1 个错误
```

## 范例四

如果 lambda 表达式引用的变量并不是当前作用域下声明的，也可以随意赋值，并不会报错

#### LambdaTester.java

```JAVA
public class LambdaTester
{
   static String salutation = "你好，";
   public static void main(String args[])
   {
      salutation = "Hello，";
      GreetingService greetService1 = message -> 
      System.out.println(salutation + message);
      greetService1.sayMessage("D");
      salutation = "你好，";
   }

   interface GreetingService {
      void sayMessage(String message);
   }
}
```

运行结果如下

```
Hello，D
```

## 总结

Java lambda 表达式可以随意引用外部变量，但如果外部变量是在当前作用域声明的，则一定不可以进行第二次赋值，哪怕是在 lambda 语句之后。