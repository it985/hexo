---
title: 四、Java8 Runnable Lambda 表达式
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 2063f33b
date: 2022-05-05 18:49:36
updated: 2022-05-18 18:47:48
---

# 四、Java8 Runnable Lambda 表达式

本章节我们重点来讲讲 Java 8 中的 `Runnable` Lambda 表达式。众所周知，Java 8 中的 `Runable` 和 `Callable` 两个接口都添加了 `@FunctionalInterface` 注解，因此我们可以直接使用 Lambda 表达式来代替它们的 `run()` 和 `call()` 方法

## Runnable 表达式

Java 8 开始支持 Lambda 表达式，所以，好像，一夜间，所有添加了 `@FunctionalInterface` 注解的方法都可以使用 Lambda 表达式来创建实例，`Runnable` 也不例外，我们可以直接使用一个 Lambda 表达式来创建它的实例

```java
Runnable r = () -> System.out.println("Hello World!");
Thread th = new Thread(r);
th.start();
```

运行结果输出为 `Hello World` ，是不是很神奇，如果没有 Lambda 表达式，那么原来的代码可能如下

```java
Runnable r = new Runnable() {
   @Override
   public void run() {
    System.out.println("Hello World!");
   }
};
Thread th = new Thread(r);
th.start(); 
```

如果我们的 Lambda 表达式需要多行代码，可以用一对打括号 `{}` 扩起来，就像下面这样

```java
Runnable r = () -> {
    Consumer<Book> style = (Book b) -> System.out.println("Book Id:"+b.getId() + ", Book Name:"+b.getName());
    list.forEach(style);
};
```

如果 Lambda 表达式需要使用到外部的参数，那么必须对参数添加 `final` 修饰符表示参数不可变更。

```java
final List<Book> list =  Arrays.asList(new Book(1, "Ramayan"), new Book(2, "Mahabharat"));
Runnable r = () -> {
    Consumer<Book> style = (Book b) -> System.out.println("Book Id:"+b.getId() + ", Book Name:"+b.getName());
    list.forEach(style);
};
```

我们还可以把 `Runnable` 表达式作为参数传递给 `Thread` 相关的方法

我们首先来定一个 `Book.java` 类

#### Book.java

```java
package com.souyunku.tech.util.runnable;
public class Book {
        public int id;
        public String name;
        public Book(int id, String name){
            this.id = id;
            this.name = name;
        }
        public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void print(){
               System.out.println("id:"+id + ", Name:"+name);
        }
} 
```

然后在一个 `Runnable` 中输出书籍的一些信息

#### Java8RunnableDemo.java

```java
package com.souyunku.tech.util.runnable;
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import com.concretepage.Book;
public class Java8RunnableDemo {
    public static void main(String[] args) {
        final List<Book> list =  Arrays.asList(new Book(1, "Ramayan"), new Book(2, "Mahabharat"));
        Runnable r1 = () -> list.forEach(Book::print);
        Thread th1 = new Thread(r1);
        th1.start();
        Runnable r2 = () -> {
            Consumer<Book> style = (Book b) -> System.out.println("Book Id:"+b.getId() + ", Book Name:"+b.getName());
            list.forEach(style);
        };
        Thread th2 = new Thread(r2);
        th2.start();
    }
} 
```

运行结果如下

```
id:1, Name:Ramayan
Book Id:1, Book Name:Ramayan
id:2, Name:Mahabharat
Book Id:2, Book Name:Mahabharat
```

同样的，我们还可以在 `ExecutorService` 中运行 `Runnable`

#### Java8RunnableDemoExecutor.java

```java
package com.souyunku.tech.util.runnable;
import java.util.Arrays;
import java.util.List;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.function.Consumer;
import com.concretepage.Book;
public class Java8RunnableDemoExecutor {
    public static void main(String[] args) {
        final List<Book> list =  Arrays.asList(new Book(1, "Ramayan"), new Book(2, "Mahabharat"));
        ExecutorService service =  Executors.newFixedThreadPool(2);
        Runnable r1 = () -> list.forEach(Book::print);
        service.execute(r1);
        Runnable r2 = () -> {
            Consumer<Book> style = (Book b) -> System.out.println("Book Id:"+b.getId() + ", Book Name:"+b.getName());
            list.forEach(style);
        };
        service.execute(r2);
    }
} 
```