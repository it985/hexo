---
title: 十一、Java 8 接口静态方法
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: e3c8b1a4
date: 2022-05-05 19:01:13
updated: 2022-05-18 18:48:22
---

# 十一、Java 8 接口静态方法

Java 8 除了给接口带来了 默认方法之外，还给接口带来了 **静态方法**。而且，Java 8 中的静态方法还可以有具体的实现。

我们都知道，在 Java 7 及之前的版本中，接口 `interface` 是不允许有静态方法声明的，例如下面的代码



#### InterfaceStaticMethodTester.java

```JAVA
public class InterfaceStaticMethodTester
{
    public static void main(String[] args)
    {
    }

    interface Greeter {
        public static void greeter(String name);
    }
}
```

在 Java 7 版本下，编译会报错

```
InterfaceDefaultMethodTester.java:8: 错误: 此处不允许使用修饰符static
        public static void greeter(String name);
                           ^
1 个错误
```

但是，Java 8 为接口引入了 **静态方法** 功能。在 Java 8 中，接口不仅可以有静态方法声明，还可以有静态方法实现，例如

#### InterfaceStaticMethodTester.java

```JAVA
public class InterfaceStaticMethodTester
{
    public static void main(String[] args)
    {
        Greeter.greeter("A");
    }

    interface Greeter
    {
        public static void greeter(String name)
        {
            System.out.println("你好，" + name);
        }
    }
}
```

运行结果如下

```
你好，A
```

但是，Java 8 的接口中的静态方法必须有自己的实现，而不能仅仅是一个接口声明，不然编译是会报错的。

例如下面的代码

#### InterfaceStaticMethodTester.java

```JAVA
public class InterfaceStaticMethodTester
{
    public static void main(String[] args)
    {
    }

    interface Greeter
    {
        public static void greeter(String name);
    }
}
```

运行结果如下

```
InterfaceStaticMethodTester.java:9: 错误: 缺少方法主体, 或声明抽象
        public static void greeter(String name);
                           ^
1 个错误
```

如果添加了 `abstract` 关键字，其实，`abstract` 关键和 `static` 关键字是不兼容的

```JAVA
public class InterfaceStaticMethodTester
{
    public static void main(String[] args)
    {
    }

    interface Greeter
    {
        public abstract static void greeter(String name);
    }
}
```

运行结果如下

```
InterfaceStaticMethodTester.java:9: 错误: 非法的修饰符组合: abstract和static
        public abstract static void greeter(String name);
                                    ^
1 个错误
```

所以，Java 8 支持接口中的静态方法，但静态方法必须有实现，不支持静态方法声明。