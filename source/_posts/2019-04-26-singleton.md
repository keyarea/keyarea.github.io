---
title: Java设计模式-单例设计模式
date: 2019-04-25 14:44:27
tags:
  Java
categories:
  Java
---

所谓的单例设计指的是一个类只允许产生一个实例化对象。

范例：编写一个简单的程序

```java
package DesignMode.单例模式;

class Singleton {
    public void print(){
        System.out.println("Hello World");
    }
}

public class TestSingleton {
    public static void main(String[] args){
        Singleton singleton = null; // 声明一个变量
        singleton = new Singleton(); // 实例化对象
        singleton.print();
    }
}
```

> 以上的程序在进行对象实例化的时候调用了Singleton类的无参构造方法，因为在Singleton类里面并没有提供任何的构造方法，所以会自动生成一个无参的，什么都不做的构造方法。但是如果说现在将Singleton类中定义一个私有参数的构造呢？

```java
package DesignMode.单例模式;

class Singleton {
    private Singleton(){}  // 任何方法都可以使用那四种权限

    public void print(){
        System.out.println("Hello World");
    }
}

public class TestSingleton {
    public static void main(String[] args){
        Singleton singleton = null; // 声明一个变量
        singleton = new Singleton(); // 实例化对象 错误 构造函数私有
        singleton.print();
    }
}
```

> 这个时候类中已经明确的提供有了一个私有的构造方法，那么默认的无参构造将一定不会自动生成，那么也就是说此时在进行对象实例化的时候一定会有错误。

> 首先一旦构造方法被私有化了，那么就表示外部无法调用构造了，也就证明外部不能产生新的实例化对象了。那么也就证明此时的类是一个相对而言封闭的状态了。    
> 可是现在如果还想调用Singleton类中的`print()`这个普通方法，那么就必须提供有实例化对象。于是根据封装的特点，可以考虑在类的内部产生一个实例化对象。

```java
package DesignMode.单例模式;

class Singleton {
    // 在类的内部是允许访问私有的结构的，所以可以在类的内部产生实例化对象
    Singleton instance = new Singleton(); // 内部产生实例化对象
    private Singleton(){}  // 任何方法都可以使用那四种权限

    public void print(){
        System.out.println("Hello World");
    }
}

public class TestSingleton {
    public static void main(String[] args){
        Singleton singleton = null; // 声明一个变量
        // singleton = new Singleton(); // 实例化对象
        // singleton.print();
    }
}
```

现在Singleton类内部的instance对象（属性）是一个普通属性，所有的普通属性必须在有实例化对象的时候才能够进行内存空间的分配，而现在外部无法产生实例化对象，所以就必须想一个办法，可以在Singelton类没有实例化对象产生的时候也可以将instance进行使用。那么自然会联想到要使用static关键字。

```java
package DesignMode.单例模式;

class Singleton {
    // 在类的内部是允许访问私有的结构的，所以可以在类的内部产生实例化对象
    static Singleton instance = new Singleton(); // 内部产生实例化对象
    private Singleton(){}  // 任何方法都可以使用那四种权限

    public void print(){
        System.out.println("Hello World");
    }
}

public class TestSingleton {
    public static void main(String[] args){
        Singleton singleton = null; // 声明一个变量
        // singleton = new Singleton(); // 实例化对象
        // singleton.print();
        singleton = Singleton.instance; // 实例化对象
        singleton.print();
    }
}
```

> 以上虽然取得了Singleton类的实例化对象，但是对于类中的属性应该使用private封装，而如果要想取得封装的属性就必须提供有一个getter方法，不过此时访问的是一个static属性，并且这个类无法在外部进行实例化对象的创建，所以要再提供有一个static方法，因为static方法也不受实例化对象的控制。

```java
package DesignMode.单例模式;

class Singleton {
    // 在类的内部是允许访问私有的结构的，所以可以在类的内部产生实例化对象
    private static Singleton instance = new Singleton(); // 内部产生实例化对象

    public static Singleton getInstance(){
        return instance;
    }

    private Singleton(){}  // 任何方法都可以使用那四种权限

    public void print(){
        System.out.println("Hello World");
    }
}

public class TestSingleton {
    public static void main(String[] args){
        Singleton singleton = null; // 声明一个变量
        // singleton = new Singleton(); // 实例化对象
        // singleton.print();
        singleton = Singleton.getInstance(); // 实例化对象
        singleton.print();
    }
}
```

首先现在的问题已经解决了，但是这样的做法的目的是什么呢？只希望类中产生唯一的一个实例化对象。

不过对于这种单例设计模式，也有两类的形式：懒汉式，饿汉式。之前的程序实际上就属于饿汉式的应用，因为不管你是否去使用Singelton类的对象，只要该类加载了，那么一定会自动创建好一个公共的instace对象。既然是饿汉式，就希望整体的操作之中只能够有一个实例化对象，所以一般会给其加上`finnal`.

范例：饿汉式的单例设计

```java
package DesignMode.单例模式;

class Singleton {
    // 在类的内部是允许访问私有的结构的，所以可以在类的内部产生实例化对象
    private final static Singleton INSTANCE = new Singleton(); // 内部产生实例化对象

    public static Singleton getInstance(){
        return INSTANCE;
    }

    private Singleton(){}  // 任何方法都可以使用那四种权限

    public void print(){
        System.out.println("Hello World");
    }
}

public class TestSingleton {
    public static void main(String[] args){
        Singleton singleton = null; // 声明一个变量
        // singleton = new Singleton(); // 实例化对象
        // singleton.print();
        singleton = Singleton.getInstance(); // 实例化对象
        singleton.print();
    }
}
```

> 特点：构造方法私有化，外部无法产生新的实例化对象，只能通过static方法获取对象。


范例：懒汉式单例

- 指的是当第一次去使用Singleton类对象的时候才会为其进行实例化的处理操作。

```java
package DesignMode.单例模式;

// 懒汉式单例

class Message {
    private static Message instance = null;

    public static Message getInstance(){
        if(instance == null) {
            instance = new Message();
        }
        return instance;
    }

    private Message(){};  // 构造函数私有化， 外部无法实例化

    public void print(){
        System.out.println("Hello Wolrd!!");
    }
}

public class SingletonDemo {
    public static void main(String[] args){
        Message message = null; // 声明一个对象
        message = Message.getInstance();
        message.print();
    }
}
```

重要的是需要将单例设计的核心组成记住，以及慢慢理解他的用法。
