---
title: 八、Java 8 Lambda 表达式 （ 下 ）范例
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 6fd40a1c
date: 2022-05-05 18:56:13
updated: 2022-05-18 18:48:07
---

# 八、Java 8 Lambda 表达式 （ 下 ）范例

经过 Java 8 Lambda 表达式 （ 上 ）- 简介 和 Java 8 Lambda 表达式 （ 中 ）- 外部参数 两个章节的学习，想必你已经对 Java 8 中的 Lambda 表达式已经非常熟悉了。

本章节我们就来回顾下上面两章节的所学知识，然后写一些范例巩固下。

## 回顾

- lambda 表达式是 java 8 引入的最重要的功能之一。
- lambda 表达式为那些只包含一个方法的接口提供了简洁代码，例如函数接口 ( `function interface` )。
- lambda 表达式还为集合的迭代提供了一种更为简单的方式。
- 对于那些使用匿名类或内部类实现的单个方法接口，在 Java 8 中，可以使用 lambda 表达式来实现。不仅功能相同，而且代码更简洁。

lambda 表达式真正的原理，是 「 为那些函数接口定义了它们包含的唯一方法，而且返回函数接口的实例 」

### lambda 表达式语法

lambda 表达式语法非常易于阅读和理解

lambda 表达式语法看起来像

```
(Argument  part)  -> Body part 
```

例如

1、 如果某个方法没有参数，并打印信息，则可能像下面这样

```
    () -> System.out.println("Your message");
```

2、 如果某个方法接受两个参数，执行一些业务逻辑并返回值，则看起来可能像下面这样

```
    (int a, int b) ->  a+b;
表达式会返回 `a+b` 的计算结果
```

3、 如果某个方法接受一个参数，并执行一些计算，则可能像下面这样

```
    (String s) ->  s + "Hello World";
```

## Java 8 lambda 表达式的一些使用范例

### 在 `Runnable` 中使用 lambda 表达式

下面这个范例中，我们将运行一个 Runnable 线程。

在 Java 8 之前，在 lambda 表达式出现前，要实现 Runnable 线程，我们需要定义一个实现 Runnable 接口的类来获取 Runnable 对象。

#### LambdaTester.java

```JAVA
public class LambdaTester
{
   public static void main(String args[])
   {
      Runnable r = new Runnable() {

         @Override
         public void run() {
            System.out.println("你好，A");
         }
      };

      Thread thread = new Thread(r);
      thread.run();
   }
}
```

运行结果如下

```
你好，A
```

现在，我们来看看如何使用 lambda 表达式实现相同的对象。

#### LambdaTester.java

```JAVA
public class LambdaTester
{
   public static void main(String args[])
   {
      Runnable r = () -> System.out.println("你好，B");

      Thread thread = new Thread(r);
      thread.run();
   }
}
```

运行结果如下

```
你好，B
```

没有对比，就没有伤害，使用 lambda 表达式真的是简单明了，代码又少。

### 在用户自定义的函数接口中使用 lambda 表达式

在下面这个范例中，我们将创建自己的函数接口，我们先用普通的方式来实现一下

#### LambdaTester.java

```JAVA
public class LambdaTester
{
   public static void main(String args[])
   {
      Calculator cal = new Calculator() {
         @Override
         public int add(int n1, int n2 ) {
            return n1 + n2;
         }
      };

      System.out.println(cal.add(5,8));
   }

   public interface Calculator {

      public int add(int n1, int n2);    
   }
}
```

运行结果如下

```
13
```

匿名内部类实现已经很简洁了。但，如果你看了 lambda 的实现，就会发现，匿名内部类还是有点复杂的

#### LambdaTester.java

```JAVA
public class LambdaTester
{
   public static void main(String args[])
   {
      Calculator cal = (n1,n2) -> n1 + n2;
      System.out.println(cal.add(5,8));
   }

   public interface Calculator {

      public int add(int n1, int n2);    
   }
}
```

运行结果如下

```
13
```

代码少的有点不像话了…..

### 在 `Comparator` 中使用 lambda 表达式

现在，我们来看看 `Comparator` 接口，了解下如何使用 lambda 表达式对包含字符串的集合进行排序。

#### LambdaTester.java

```JAVA
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
public class LambdaTester {

    public static void main(String[] args)
    {
        List<String> list = Arrays.asList("Ram","Shyam","Kabir");
        System.out.println("...排序之前...");
        for(String s : list){
            System.out.println(s);
        } 


        //定义一个 comparator
        Comparator<String> comp= (String s1, String s2) -> s1.compareTo(s2);
        Collections.sort(list, comp);

        System.out.println("...排序之后...");
        for(String s : list){
            System.out.println(s);
        }
    }
}
```

运行结果如下

```
...排序之前...
Ram
Shyam
Kabir
...排序之后...
Kabir
Ram
Shyam
```

### 使用 Lambda 表达式实现函数接口来迭代集合

Java 8 中引入了 `java.util.function` 包。这个包提供了 `java.util.function.Function` 接口，我们可以在这个接口的帮助下，使用 Lambda 表达式来迭代集合。

`java.util.function.Function` 只有一个方法 `apply()` ，这个方法可以在用户定义的函数中调用。我们可以使用 `Function` 接口实现一个自定义的打印方法

```JAVA
public String customShow( Function<String,String> fun) {
    return fun.apply(this);
}
```

然后，我们就可以使用 Lambda 来实现这个接口了

#### LambdaTester.java

```JAVA
import java.util.ArrayList;
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
        List<LambdaTester.Student> list = new ArrayList<LambdaTester.Students>();

        list.add(new LambdaTester.Student("Ram",20));
        list.add(new LambdaTester.Student("Shyam",22));
        list.add(new LambdaTester.Student("Kabir",18));
        for(Student st: list){
            System.out.println(st.customShow(s->s.getName()+": "+s.getAge()));
        }
    }

    public class Student
    {
        private String name;
        private int age;
        public Student(String name,int age){
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

        public int getAge() {
            return age;
        }

        public void setAge(int age) {
            this.age = age;
        }

        public  String customShow(Function<Student,String> fun){
            return fun.apply(this);
        }
    }
}
```

运行结果如下

```
Ram: 20
Shyam: 22
Kabir: 18
```