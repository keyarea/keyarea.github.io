---
title: Java中的注解
date: 2019-04-25 14:44:27
tags:
  Java
categories:
  Java
---
# annotation(注解)

Annotation是jdk1.5之后提出的一个新的开发技术结构，利用Annotation可以有效减少程序配置的代码，
并且可以利用Annotation进行一些结构化的定义。

Annotation是以一种注解的形式实现的程序开发。

如果要想清除Annotation的产生意义，则必须了解一下程序开发结构的历史，从历史来讲，程序开发一共分为了
三个过程：

> 过程一：在程序定义的时候将所有可能使用到的资源全部定义在代码之中；
> - 如果此时服务器的相关的地址发生了改变，那么对于程序而言，就需要进行源代码的修改了，维护需要开发人员来完成。
这样的做法明显是不方便的。

> 过程二：引入配置文件，在配置文件中定义全部要使用的服务器资源。
> - 在配置项不多的情况下，此类配置非常好用，并且十分的简单，但是如果这个时候所有的项目都是采用这种结构开发，那么就
可能出现一种可怕的场景：配置文件太多；
> - 所有的操作都需要通过配置文件完成，这样对于开发的难度提升了；

> 过程三：将配置信息重新写回到程序里面，利用一些特殊的标记与程序代码进行分离，这就是注解的作用，
也就是annotation提出的基本依据。
> - 如果全部使用注解进行开发，难度太高了，配置文件有好处也有缺点，所以现在的开发是围绕着配置文件+注解的形式完成的；


基本的几个注解：@Override , @Deprecated , @SuppressWarnings。

## 基本注解

### @Override 覆写

当子类继承某一个父类之后发现父类中的某些方法功能不足的时候，往往会采用覆写的形式来对我们功能进行扩充，于是下面首先来观察一种覆写操作。

范例：[观察覆写的问题](TestDemo.java)

```java
class Channel{
    public void content(){
        System.out.println("*********** Channel *************");
    }
}

class DatabaseChannel extends Channel{
    public void content(){
        System.out.println("************ DatabaseChannel ************");
    }
}

public class TestDemo {
    public static void main(String[] args){
        new DatabaseChannel().content();
    }
}
```

> 开发之中经常出现的两个问题：
> - 虽然明确的继承一个父类，并且进行父类的覆写，但是有可能由于疏忽忘记编写extends，于是这个时候不是覆写；
> - 在进行方法覆写的时候单词写错了。

> 此时即便是以上的两个问题都发生了，实际上程序在编译的时候也不会出现任何的错误信息，因为它认为这是一个新的方法。
>
> 所以在开发之中为了避免这种问题的出现，可以在明确覆写的方法上追加有一个注解。

范例：[追加注解](TestDemo1.java)

```java
class Person{
    public void say(){
        System.out.println("我是一个人");
    }
}

class Man extends Person{
    @Override  // 明确标识这是一个覆写操作
    public void say(){
        System.out.println("我是一个男人");
    }
}

public class TestDemo1 {
    public static void main(String[] args){
        new Man().say();  // 我是一个男人
    }
}
```

该注解主要是帮助开发者在程序编译的时候可以检查出程序的错误。

### @Deprecated 过期操作

所谓的过期操作指的是在一个软件项目的迭代开发过程之中，可能有某一个方法或者某一个类，由于在最初设计的时候考虑不周（存在有缺陷），导致新版本的应用会有不适应的地方（老版本不影响），
这个时候又不可能直接删除掉这些操作，那么就希望给一个过渡的时间，于是就可以采用过期的声明，目的告诉新的用户这些操作不要再用了，老的用户你用就用了。
这些方法就必须利用我们的`@Deprecated`注解进行定义。

范例：[声明过期操作](TestDemo2.java)

```java
class Women{
    @Deprecated // 过期的方法，不建议使用
    public void say(){
        System.out.println("你好");
    }

    public String speak(){
        return "hello";
    }
}

public class TestDemo2 {
    public static void main(String[] args){
        new Women().say();
    }

}
```

在有一些开源项目里面特别讨厌的一点：新版本出现之后，将里面的方法彻底变个样。

### @SuppressWarnings 压制警告

以之前的过期程序为例，可以发现在进行程序编译的时候会出现一个错误的提示信息。

> 如果此时不愿意见到这些错误信息（或者已经明确了错误在那里），那么就可以进行警告信息的压制

```java
class Women{
    @Deprecated // 过期的方法，不建议使用
    public void say(){
        System.out.println("你好");
    }

    public String speak(){
        return "hello";
    }
}


public class TestDemo2 {
    @SuppressWarnings({"deprecation"})
    public static void main(String[] args){
        new Women().say();
    }

}
```

它要做的只是让警告信息不出现，不骚然你而已。
