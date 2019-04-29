---
title: Java 异常的捕获与处理
date: 2019-04-29 14:44:27
tags:
  Java
categories:
  Java
---

几乎所有的代码里面都有可能产生异常，所以为了保证程序出现异常之后可以正常执行完毕，就需要进行处理。

# 异常的影响

异常是导致程序中断执行的一种指令流。但是程序之中如果出现异常并且没有合理处理的话，就会导致程序终止执行。

范例：观察没有异常产生的程序

```java
package 异常与处理;

public class NoError {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        System.out.println("[2] 数学计算中：result = " + (10 / 3));
        System.out.println("[3] 数学计算后");
    }
}
```

如果此时没有任何异常产生，那么程序可以正常的执行完毕。

范例：观察产生异常

```java
package 异常与处理;

public class HasError {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        System.out.println("[2] 数学计算中：result = " + (10 / 0)); // 错误 分母不能为0
        System.out.println("[3] 数学计算后");
    }
}
```

异常：

```shell
[1] 数学计算前
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at 异常与处理.HasError.main(HasError.java:6)
```

> 程序之中产生了异常，但是在异常语句之前的代码依然可以正常执行完毕，而异常产生之后程序将直接进行了结束。

> 为了保证程序出现异常后还可以继续向下执行，就需要进行异常处理。

# 异常的处理

> 为了保证程序出现错误之后依然能够正常的执行，那么就可以使用异常的处理操作，异常处理的代码格式如下：

```java
try {
    有可能出现异常的语句;
}[catch (异常类 对象){
    异常的处理语句;
}catch (异常类 对象){
    异常的处理语句;
} ......][
finally {
    异常的统一出口;
}]
```

> 对于以上的三个关键字，可以出现的组合：`try...catch`、`try...finally`、`try...catch...finally`。

范例：对异常进行处理

```java
package 异常与处理;

public class ExceptionHandle {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        try{
            System.out.println("[2] 数学计算中：result = " + (10 / 0)); // 错误 分母不能为0
        } catch (ArithmeticException e){
            System.out.println("[catch] 异常已经被处理！");
        }
        System.out.println("[3] 数学计算后");
    }
}
```

> 发现出现了异常之后，整个的程序由于存在有异常的处理机制，那么依然可以正常的执行完毕。

> 以上的代码虽然进行了异常的处理，但是这里面会存在有一个严重的问题，也就是说你现在根本就不知道你的程序产生了什么样的异常，所以为了明确的取得异常的信息，可以直接输出异常类对象，或者直接调用所有异常类中提供的`printStackTrace()`,进行完整异常信息的输出。

范例：取得异常的完整信息

```java
package 异常与处理;

public class ExceptionHandle {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        try{
            System.out.println("[2] 数学计算中：result = " + (10 / 0)); // 错误 分母不能为0
        } catch (ArithmeticException e){
            e.printStackTrace();
        }
        System.out.println("[3] 数学计算后");
    }
}
```

> 在进行异常信息处理的时候还可以使用`try...catch...finally`来进行处理。


范例：观察`try...catch...finally`操作

```java
package 异常与处理;

public class ExceptionHandle {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        try{
            System.out.println("[2] 数学计算中：result = " + (10 / 0)); // 错误 分母不能为0
        } catch (ArithmeticException e){
            e.printStackTrace();
        } finally {
            System.out.println("[finally] 不管是否有异常都一定执行此语句。");
        }
        System.out.println("[3] 数学计算后");
    }
}
```

> 不管此时是否产生有异常，最终都要执行finally程序代码，所以finally会作为程序的统一出口。    
> 以上的程序是直接固定好了两个数字进行除法运算，现在希望通过初始化参数来进行除法计算。

范例：接收参数进行计算

```java
package 异常与处理;

public class ExceptionHandle {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        try{
            int x = Integer.parseInt(args[0]);
            int y = Integer.parseInt(args[1]);
            System.out.println("[2] 数学计算中：result = " + (x / y));
        } catch (ArithmeticException e){
            // System.out.println("[catch] 异常已经被处理！");
            e.printStackTrace();
        } finally {
            System.out.println("[finally] 不管是否有异常都一定执行此语句。");
        }
        System.out.println("[3] 数学计算后");
    }
}
```

> 那么下面来执行以上的程序，正确的执行：`java ExceptionHandle 10 2`    
> 但是这个时候有可能会存在有如下的问题：
> - 用户执行的时候没有输入参数：`java ExceptionHandle`：`ArrayIndexOutOfBoundsException`
> - 用户执行的时候没有输入参数：`java ExceptionHandle a b`：`NumberFormatException`
> - 被除数为0：`java ExceptionHandle 10 0`

> 通过以上代码可以发现，原来在进行catch捕获异常的时候，如果没有捕获指定的异常，那么程序依然无法进行处理，所以现在最直白的解决方案就是使用多个catch:

```java
package 异常与处理;

public class ExceptionHandle {
    public static void main(String[] args){
        System.out.println("[1] 数学计算前");
        try{
            int x = Integer.parseInt(args[0]);
            int y = Integer.parseInt(args[1]);
            System.out.println("[2] 数学计算中：result = " + (x / y));
        } catch (ArithmeticException e){
            // System.out.println("[catch] 异常已经被处理！");
            e.printStackTrace();
        } catch (NumberFormatException e){
            e.printStackTrace();
        } catch (IndexOutOfBoundsException e){
            e.printStackTrace();
        }
        finally {
            System.out.println("[finally] 不管是否有异常都一定执行此语句。");
        }
        System.out.println("[3] 数学计算后");
    }
}
```

问题是如果真这么写，那么测试，不使用异常处理也一样，因为完全可以使用if...else判断。如果想要更好的处理异常，那么你必须清除异常的处理流程。

# throw关键字

throw是直接编写在语句之中的，表示人为进行异常的抛出。例如，在之前使用过了一个`10/0`这样的语言，而这样的语句执行之后所产生的数学异常是由JVM负责进行异常类的对象实例化的。而现在如果不希望异常类对象由我们的系统产生，希望由用户来控制异常类实例化对象的产生，就可以使用`throw`来完成。

范例：使用throw来产生异常类实例化对象

```java
package 异常与处理;

public class ThrowException {
    public static void main(String[] args){
        try{
            throw new Exception("一个异常"); // 抛出一个异常
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

> 一般而言throw和break、continue、return一样，都需要结合if判断来执行。

面试题：请解释throw和throws的区别？

- throw用于方法内部主要表示进行手工的异常抛出。
- throws主要在方法声明上使用，明确的告诉用户本方法可能产生的异常，同时该方法可能不处理此异常。

# 异常处理模型

**综合案例：异常处理标准格式**

现在为止异常中的所有核心概念都掌握了：try、catch、finally、throw、throws、

现在要求编写一个方法--进行除法操作，但是对于此方法的要求如下：

- 在进行除法计算操作之前首先要打印一行语句；
- 在进行除法计算的过程中出现错误，则应该将异常返回给调用处；
- 不管追中是否有错误的产生，都要求打印一行计算结束的信息；

范例：观察程序的实现 

```java
package 异常与处理;

class MyMath{

    // 如果该方法不进行异常的处理，一定要使用throw进行抛出
    public static int div(int x, int y)
    throws Exception{
        int result = 0;
        System.out.println("除法计算前");
        try{
            result = x / y; // 此处有异常，后面不进行处理
        }catch (Exception e){
            throw e; // 将错误抛出去
        }finally {
            System.out.println("除法计算后");
        }

        return result;
    }
}

public class TestDemo {
    public static void main(String[] args){
        try {
            System.out.println(MyMath.div(10, 0));
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

> 当然，对于以上的格式还可以进行进一步简化：直接使用`try...finally`。

```java
package 异常与处理;

class MyMath{

    // 如果该方法不进行异常的处理，一定要使用throw进行抛出
    public static int div(int x, int y)
    throws Exception{
        int result = 0;
        System.out.println("除法计算前");
        try{
            result = x / y; // 此处有异常，后面不进行处理
        }
        finally {
            System.out.println("除法计算后");
        }

        return result;
    }
}

public class TestDemo {
    public static void main(String[] args){
        try {
            System.out.println(MyMath.div(10, 0));
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

# RuntimeException类

首先看一段简单的程序。

```java
public class TestDeom {
    public static void main(String[] args){
        String str = '100';
        int num = Integer.parseInt(str);
        System.out.println(num * 2);
    } 
}
```

来观察一下Integer类中关于parseInt()方法的定义

```java
public static int parseInt(String s)
                   throws NumberFormatException
```

这个方法上已经明确的跑出了一个异常，但是在进行调用的时候发现，即使没有进行异常处理，也可以正常执行，这个就属于`RuntimeException`的范畴了，来观察`NumberFormException`的继承结构：

```
java.lang.Object
  java.lang.Throwable
      java.lang.Exception
          java.lang.RuntimeException
              java.lang.IllegalArgumentException
                  java.lang.NumberFormatException
```

很多的代码上都可能会出现异常，例如：数学计算：‘10/0’都可能会产生异常，那么如果所有有可能产生异常的地方都进行强制性的异常处理，这个代码就太复杂了。所以在进行异常设计的时候考虑到一些异常可能是一些简单的问题，所以将这类的异常统一称为`RuntimeException`，也就是说使用`RuntimeException`定义的异常可以不需要强制性进行异常处理。

面试题：请解释Exception与RuntimeException的区别？请列举出几个常见的RuntimeException？

- Exception是RuntimeException的父类，使用Exception定义的异常都要求必须使用异常处理；
- RuntimeException可以由用户选择性的来进行异常处理；
- 常见的`RuntimeException`：`ClassCastException`、`IndexOutOfBoundsException`、`NullPointerException`、`ArithmeticException`；

# 断言：assert

断言是从jdk1.4引入的一个概念。所谓断言的概念指的是当我们的程序执行到某些语句之后其某数据的内容一定是约定的内容。

范例：观察断言

```java
package 异常与处理;

public class AssertDemo {
    public static void main(String[] args){
        int num = 0;
        // 中间经过很多步骤，预计num的值变为300
        assert num == 100 : "错误：num的值不为100";
        System.out.println(num);
    }
}
```

> 如果要想让断言起作用，则必须使用一个`-ea`的参数：`java -ea AssertDemo`；

其实断言的意义并不是很大，java之所以引入主要是为了与C++相同。

# 自定义异常类

在Java里面实际上针对可能出现的公共程序问题都会提供有相应的异常信息，但是很多时候这些异常信息往往不够我们去处理的；现在在进行加法处理的时候，如果发现两个内容的相加结果为30，那么就应该抛出一个`AddException`的异常。但是这样的异常java不会提供的，所以就必须定义一个属于自己的异常类。

如果想要定义属于自己的异常类可以继承两种父类：`Exception`、`RuntimeException`；

范例：实现自定义异常类

```java
package 异常与处理;

class AddException extends Exception{
    public AddException(String msg){
        super(msg);
    }
}

public class CustomException {
    public static void main(String[] args)
    throws Exception{
        int x = 10;
        int y = 20;
        if((x+y) == 30){
            throw new AddException("错误的相加操作");
        }
    }

}
```

一般你如果做一些项目的系统设计的时候，会用到自定义异常类。

# 需要注意的

- 异常的处理流程；
- 异常的处理格式；
- 异常处理的模型。