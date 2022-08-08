---
title: 十四、Java 8 Nashorn JavaScript
top_img: img/hzw.webp
cover: img/hzw.webp
categories:
  - Java
tags:
  - Java8
abbrlink: 9a637408
date: 2022-05-05 19:05:22
updated: 2022-05-18 18:48:34
---

# 十四、Java 8 Nashorn JavaScript

对于 Java 中的 JavaScript 引擎， Java 8 引入了 `Nashorn` 来代替原先的 `Rhino`。

`Nashorn` 使用 Java 7 中引入的调用动态特性，且直接编译内存中的代码并将字节码传递给 JVM。这两项改进，直接给 `Nashorn` 带了至少 2 到 10 倍的性能提升。

## jjs

在 `Nashorn` JavaScript 引擎中。JAVA 8 引入了一个新的命令行工具 `jjs`，用于在控制台执行 javascript 代码。

例如我们可以在当前目录下 ( 任意位置 ) 创建一个 JavaScript 文件 `hello.js` ，然后输入以下内容

```
print('你好，A，B ！');
```

保存文件后，使用下面的 jjs 命令运行它

```
jjs hello.js
```

运行结果如下

```
$ jjs hello.js
你好，A，B ！
```

## jjs 交互式解释器

我们也可以直接在命令行中输出 `jjs` 进入 Java 8 提供的 `jjs` 交互式解释器。

```
$ jjs
jjs> 
```

然后输入一些 JavaScript 语句，就会立即显示结果

```
jjs> print('你好，A，B ！');
你好，A，B ！
jjs> 5 + 8
13
jjs> 8 / 5
1.6
jjs> 
```

## 给 jjs 交互式解释器传递参数

在启动 jjs 交互式解释器时，我们还可以使用两个中划线 ( `--` ) 来传递一些参数。

参数之间使用空格分隔 ( 注意，多个空额会视为单个空格 )。

例如

```
$ jjs -- 你好 A B 
jjs> print('你输入的是: ' +arguments.join(", "))
你输入的是: 你好, A, B
jjs> 
```

## 在 Java 中调用 JavaScript

通过 `ScriptEngineManager` 类，Java 可以解析和调用 JavaScript 代码。

`ScriptEngineManager` 类在 `javax.script` 包中。

Java 中调用 javascript 代码的步骤一般如下

1、 创建 JavaScript 引擎的管理器，也就是创建一个 `ScriptEngineManager` 类的实例。

```JAVA
    ScriptEngineManager scriptEngineManager = new ScriptEngineManager();
```

2、 通过 JavaScript 引擎的管理器获取一个 JavaScript 引擎，比如 Java 8 中引入的 `Nashorn`。

```JAVA
    ScriptEngine nashorn = scriptEngineManager.getEngineByName("nashorn");
```

3、 调用获取到的引擎的 `eval()` 方法执行一些代码。

```
    result = (Integer) nashorn.eval("10 + 2");
需要注意的是，执行 JavaScript 代码可能会抛出 ScriptException 异常。因此要放在一个 `try catch` 语句中。
```

上面这三个流程，会涉及到三个类 `ScriptEngineManager`、`ScriptEngine` 和 `ScriptException`。它们都在 `javax.script` 包中。

```JAVA
import javax.script.ScriptEngineManager;
import javax.script.ScriptEngine;
import javax.script.ScriptException;
```

我们写一个范例演示下这个流程

#### Java8Tester.java

```JAVA
import javax.script.ScriptEngineManager;
import javax.script.ScriptEngine;
import javax.script.ScriptException;

public class Java8Tester {

   public static void main(String args[]) {
      ScriptEngineManager scriptEngineManager = new ScriptEngineManager();
      ScriptEngine nashorn = scriptEngineManager.getEngineByName("nashorn");

      String name = "A";
      Integer result = null;

      try {
         nashorn.eval("print('" + name + "')");
         result = (Integer) nashorn.eval("8 + 5 * 3");

      } catch(ScriptException e) {
         System.out.println("脚本执行失败: "+ e.getMessage());
      }
      System.out.println(result.toString());
   }
}
```

运行结果如下

```
A
23
```

## 在 JavaScript 中调用 Java 代码

如果在 JavaScript 中调用 Java 代码，我们就不做过多介绍了，因为介绍起来是一个庞大的工程，简直就是另外一门语言的赶脚。

直接看一个示例。

把创建一个 hello.js 文件，然后输入以下内容

#### hello.js

```JS
var BigDecimal = Java.type('java.math.BigDecimal');

function calculate(amount, percentage) {

   var result = new BigDecimal(amount).multiply(new BigDecimal(percentage)).divide(
      new BigDecimal("100"), 2, BigDecimal.ROUND_HALF_EVEN);

   return result.toPlainString();
}
var result = calculate(568000000000000000023,13.9);
print(result);
```

然后我们使用 `jjs` 命令来运行它

```
$ jjs hello.js 
78952000000000002017.94
```