---
title: Java 基础类库
date: 2019-09-02 22:18:23
tags:
- Java
category:
- Java
---

# Java基础类库

## StringBuffer类

String类是项目开发之中一定会使用到的一个功能类，并且这个类拥有如下的特点：

- 每一个字符串常量都属于一个String类的匿名对象，并且不可更改；
- String有两个常量池：静态常量池、运行时常量池；
- String类对象的实例化建议使用直接赋值的形式完成，这样可以直接将对象保存在对象池之中以方便下次重用；

虽然String类很好用，但是如果认真去思考也会发现其最大的弊端：内容不允许修改，虽然大部分情况下都不会涉及到字符串内容的频繁修改，但是依然可能会出现这样的情况，所以为了解决此问题，所以专门提供了StringBuffer类，
可以实现字符串内容的修改处理。

StringBuffer并不像String类那样拥有两种对象实例化方式，StringBuffer必须像普通类对象那样首先进行对象的实例化，而后才可以调用方法去执行，而这个时候
可以考虑使用StringBuffer类中的如下方法：

- 构造方法：public StringBuffer()
- 构造方法：public StringBuffer(String str) ,接收初始化字符串内容
- 数据追加: public StringBuffer append(数据类型 变量): 相当于字符串的加操作；

范例：观察String与StringBuffer的对比

```java
package Java基础类库;

/**
 * String类的引用传递
 */

public class JavaApiDemo {
    public static void main(String[] args) {
        String hello = "Hello";
        change(hello);
        System.out.println(hello); // hello字符串并没有发生改变
    }

    public static void change(String temp) {
        temp += " World";
    }
}
```

实际上大部分情况下，很少会出现字符串内容的改变，这种改变并不是针对于静态常量池的改变。

范例： 分析一下已有问题

```java
package Java基础类库;

public class JavaApiDemo2 {
    public static void main(String[] args) {
        String str1 = "Hello World";
        String str2 = "Hello " + "World";
        System.out.println(str1 == str2); // true
    }
}
```

> 这个时候的str2并不算是改变，或者更加严格的意义上来讲，对于现在str2当程序编译之后会变为如下的形式：

```
StringBuffer bug = new StringBuffer();
buf,append("Hello ").append("World"):
```

所有的”+“方法在编译之后都变为了StringBuffer中的append()方法，并且在程序之中StringBuffer与String类对象之间本来就可以直接互相转换：

- String类对象变为StringBuffer类对象可以依靠StringBuffer类的构造方法或者使用append方法；
- 所有的类对象都可以通过toString()方法将其变为String类型。

在StringBuffer类里面除了可以支持有字符串内容的修改之外，实际上也提供有String类所不具备的方法：

- 插入数据： public  StringBuffer insert(int offset, 数据类型 b);
- 删除指定范围的数据： public StringBuffer delete(int start, int end);
- 字符串反转： public StringBuffer reverse();

实际上还有一个与StringBuffer类类似的功能类：StringBuilder类，这个类是在jdk1.5之后提供的。该类与StringBuffer类提供的功能相同，最大的区别在于StringBuffer类
中的方法属于线程安全的，全部使用了synchronized关键字进行标注，而StringBuilder属于非线程安全的。

- String类是字符串的首选类型，其最大的特点是不允许修改；
- StringBuffer和StringBuilder类的内容允许修改；
- StringBuffer是在JDK1.0的时候提供的，属于线程安全的操作，而StringBuilder是在JDK1.5之后提供的，属于非线程安全的操作。

## CharSequence接口

> CharSequence是一个描述字符串结构的接口,在这个接口中我们可以发现三种常用的子类:

> String类

```java
public final class String
extends Object
implements Serializable, Comparable<String>, CharSequence
```

> StringBuffer类

```java
public final class StringBuffer
extends Object
implements Serializable, CharSequence
```

> StringBuilder类

```java
public final class StringBuilder
extends Object
implements Serializable, CharSequence
```

![](http://imgs.loong.io/20190901133929_g9g485_Screenshot.jpeg)

> 现在只要有字符串就可以为CharSequence接口实例化.

```java
public class CharSequenceTest {
    public static void main(String[] args) {
        CharSequence str = "Hello"; // 子类实例向父类接口转型
        System.out.println(str);
    }
}
```

> CharSequence本身是一个接口,在该接口中也定义有如下的操作方法:

- 获取指定索引字符:`public char charAt(int index)`;
- 获取字符串的长度: `public int length()`;
- 截取部分字符串: `public CharSequence subSequence(int start, int end)`;


范例: 字符串的截取

```java
public class CharSequenceTest1 {
    public static void main(String[] args) {
        CharSequence str = "helloworld";
        CharSequence sub = str.subSequence(5, 10);
        System.out.println(sub); // world
    }
}
```

> 以后看见CharSequence描述的就是一个字符串.

## AutoCloseable接口

> AutoCloseable主要是用于日后进行资源开发的处理上,以实现资源的自动关闭(释放资源),例如:在以后进行文件,
网络,数据库的开发过程之后由于服务器的资源有限,所以实用之后一定要关闭资源,这样才可以被更多的使用者所使用.
>
> 下面为了更好的说明资源的问题,将通过一个消息的发送处理来完成. 

范例: 手工实现资源的处理

```java
public class AutoCloseableTest {
    public static void main(String[] args) {
        NetMessage msg = new NetMessage("hello world");
        msg.send();
        msg.close();
    }
}

interface IMessage {
    public void send();
}

class NetMessage implements IMessage {
    private String msg;

    public NetMessage(String msg) {
        this.msg = msg;
    }

    public boolean open() {
        System.out.println("[open]获取消息发送链接资源");
        return true;
    }

    public void close() {
        System.out.println("[close]关闭消息发送链接资源");
    }


    @Override
    public void send() {
        if (this.open()) {
            System.out.println("[send]发送消息" + this.msg);
        }
    }
}
```

> 既然所有的资源处理完成之后都必须进行关闭操作,那么能否实现一种自动关闭的功能呢?
>
> 在这种的情况下,推出了AutoCloseable访问接口,这个接口是在JDK1.7的时候提供的,并且该接口只提供有一个方法:
> - 关闭方法: `public void close() throws Exception;`


![](http://imgs.loong.io/20190901142730_jYHVWN_Screenshot.jpeg)

> 要想实现自动关闭的处理,除了要使用AutoCloseable之外,还需要结合异常处理语句完成才可以正常调用.

范例: 实现自动关闭处理

```java
public class AutoCloseableTest {
    public static void main(String[] args) {
        try(IMessage msg = new NetMessage("hello")) {
            msg.send();
        }catch (Exception e) {

        }
    }
}

interface IMessage extends AutoCloseable{
    public void send();
}

class NetMessage implements IMessage {
    private String msg;

    public NetMessage(String msg) {
        this.msg = msg;
    }

    private boolean open() {
        System.out.println("[open]获取消息发送链接资源");
        return true;
    }

    public void close() {
        System.out.println("[close]关闭消息发送链接资源");
    }


    @Override
    public void send() {
        if (this.open()) {
            System.out.println("[send]发送消息" + this.msg);
        }
    }
}
```

> 在以后接触到资源的关闭问题,往往都会见到AutoCloseable接口的使用.

## Runtime类

> Runtime描述的是运行时的状态,也就说在整个的JVM之中,Runtime是唯一一个与JVM运行状态有关的类,并且来讲都会默认提供有一个该类的实例化对象.
>
> 由于在每一个JVM进程里面只允许提供有一个Runtime类的对象,所以这个类的构造方法被默认私有化了,那么就证明该类使用的是单例设计模式,并且每一个单例设计模式一定会提供有一个static方法获取本类
实例.

![](http://imgs.loong.io/20190901144631_8WimLJ_Screenshot.jpeg)

> 由于Runtime类属于单例设计模式,如果想要获取实例化对象,那么就可以依靠类中的getRuntime()方法完成
> - 获取实例化对象: `public static Runtime getRuntime();`
>
> 通过Runtime类中的availableProcessors()方法可以获取本机的CPU内核数

范例: 获取Runtime对象

```java
public class RuntimeTest {
    public static void main(String[] args) {
        Runtime run = Runtime.getRuntime(); // 获取实例化对象
        System.out.println(run.availableProcessors()); // 12
    }
}
```

> 除了以上的方法之外,在Runtime类中还提供有如下的四个重要方法:
> - 获取最大可用内存空间: `public long maxMemory()`, 默认配置为系统最大内存的四分之一;
> - 获取可用内存空间: `public long totalMemory()`, 默认为系统的64份之一;
> - 获取空闲内存空间: `public long freeMemory()`;
> - 手工进行GC处理: `public void gc()`;

范例: 观察内存的使用

```java
public class RuntimeTest2 {
    public static void main(String[] args) {
        Runtime run = Runtime.getRuntime();
        System.out.println("[1]MAX_MEMORY: " + run.maxMemory());
        System.out.println("[1]TOTAL_MEMORY: " + run.totalMemory());
        System.out.println("[1]FREE_MEMORY: " + run.freeMemory());

        String str = "";
        for (int i = 0;i < 2000; i++) {
            str += i; // 产生大量垃圾空间
        }

        System.out.println("[2]MAX_MEMORY: " + run.maxMemory());
        System.out.println("[2]TOTAL_MEMORY: " + run.totalMemory());
        System.out.println("[2]FREE_MEMORY: " + run.freeMemory());

        run.gc();

        System.out.println("[3]MAX_MEMORY: " + run.maxMemory());
        System.out.println("[3]TOTAL_MEMORY: " + run.totalMemory());
        System.out.println("[3]FREE_MEMORY: " + run.freeMemory());
    }
}
```

> GC(Garbage Collector)垃圾收集器, 是可以由系统自动调用的垃圾释放功能,或者使用Runtime类中的gc()手工调用.

## System类

> System是一直陪伴着我们的程序类,之前的输出就是采用System类中的方法,在System类中也定义有一些其他的处理方法:
> -  数组拷贝:`public static void arraycopy(Object src,int srcPos,Object dest,int destPos,int length)`;
> - 获取当前的时间日期数值: `public static long currentTimeMillis()`;
> - 进行垃圾回收: `public static void gc()`;

范例: 操作耗时的统计

```java
public class SystemTest {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        String str = "1";
        for (int i = 0; i< 30000; i++) {
            str += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("操作耗时: " + (end - start));
    }
}
```

> 在System类中也提供有一个gc()方法,但是这个gc()方法并不是定义的新方法,而是继续调用了Runtime类中的gc()方法(Runtime.getRuntime().gc()).

## Cleaner类

> Cleaner类是在JDK1.9之后提供的一个对象的清理操作,其主要的功能是进行finalize()方法的替代.在C++中有两种特殊的函数:构造函数,析构函数(对象的手工回收),
在Java之中所有的垃圾空间都是通过GC自动回收的,所以很多情况下是不需要使用这类析构函数的,也正是因为如此,所以Java并没有提供这方面的支持.
>
> 但是Java本身依然给用户提供了收尾的操作,每一个实例化对象在回收之前至少给他一个喘息的机会,最初实现对象收尾处理方法是Object类中所提供的finalize()方法,这个方法的定义如下:


```java
@Deprecated(since="9")
protected void finalize() throws Throwable
```

> 已经不建议继续使用这个方法了,而是说子类可以继续使用这个方法名称,但是这个方法上最大的特征就是抛出了一个Throwable异常类型,而这个异常类型分为两个子类型:Error, Exception,平常所处理的一般就是Exception.

范例: 观察传统的回收操作

```java
public class FinalizeTest {
    public static void main(String[] args) {
        Member m = new Member();
        m = null; // 成为垃圾
        System.gc();
        System.out.println("照常运行");
    }
}

class Member {
    public Member() {
        System.out.println("类诞生了!");
    }

    @Override
    protected void finalize() throws Throwable {
        System.out.println("类回收");
        throw new Exception("我还活着");
    }
}
```

> 但是从JDK1.9开始,这一操作已经不建议使用了,而对于对象的回收释放.从JDK1.9开始建议开发者使用AutoCloseable或者使用java.lang.ref.Cleaner类进行回收处理(Cleaner也支持AutoCloseable的处理).

```java
package Java基础类库;

import java.lang.ref.*;

class Example implements Runnable{
    public Example (){
        System.out.println("该类被创建了");
    }

    @Override
    public void run() {
        System.out.println("我还活着");
    }
}

class ExampleCleaner implements AutoCloseable {
    private static final Cleaner cleaner = Cleaner.create();
    private static Example example;
    private Cleanable cleanable;

    public ExampleCleaner () {
        this.example = new Example(); // 创建新对象
        this.cleanable = this.cleaner.register(this, this.example); // 注册使用的对象
    }
    @Override
    public void close() throws Exception {
        this.cleanable.clean(); // 启动多线程
    }
}

public class CleanerTest {
    public static void main(String[] args) {

        try(ExampleCleaner ec = new ExampleCleaner()) {
            // 执行相关代码
        }catch (Exception e){}
    }
}
```

> 在新的清除回收处理的过程之中,更多的情况下考虑的是多线程的使用,即为了防止有可能造成的延迟处理,所以许多对象回收前的处理都是单独通过一个线程完成的.

## 对象克隆

> 所谓的对象克隆指的就是对象的复制,而且属于全新的复制.即:使用已有的对象内容创建一个新的对象,如果要想进行对象克隆需要使用到Object类中提供的clone()方法:

```java
protected Object clone() throws CloneNotSupportedException
```

> 所有的类都会继承Object父类,所以所有的类都一定会有clone方法,但是并不是所有的类都希望被克隆.所以如果要想实现对象克隆,那么对象所在的类需要实现一个Cloneable接口,此接口并没有任何的方法提供,
是因为它描述的是一种能力.

范例: 实现对象克隆

```java
public class CloneTest {
    public static void main(String[] args) throws Exception {
        Person p = new Person("王五", 25);
        Person p1 = (Person) p.clone();
        System.out.println(p1);
        System.out.println(p);
    }
}

class Person implements Cloneable{
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "[" + super.toString() + "]" + "name:" + this.name + " age: " + this.age;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

> 在实际的开发之中不是非常特殊的情况下,很少会出现对象克隆的需求.