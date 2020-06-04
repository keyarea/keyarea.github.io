---
title: Java 线程的同步与死锁
date: 2019-09-02 22:27:09
tags:
- Java
- Thread
category:
- Java
---

# 线程的同步与死锁

在多线程的处理之中,可以利用Runnable描述多个线程操作的资源,而Thread描述每一个线程对象,于是当多个线程
当同一线程访问同一资源的时候如果处理不当就会产生数据的错误操作.

## 同步问题的引出

下面编写一个简单的卖票程序,将创建若干个线程对象实现卖票的处理操作.

范例: 卖票处理操作

```java
package 线程.线程的同步与死锁;

class SaleTicket implements Runnable {
    private int ticket = 10;
    @Override
    public void run() {
        while (true) {
            if(this.ticket > 0) {
                System.out.println(Thread.currentThread().getName() + "卖票, 还有票" + this.ticket --);
            }else{
                System.out.println("票已经卖光了!!");
                break;
            }
        }
    }
}

public class ThreadDemo1 {
    public static void main(String[] args) {
        SaleTicket st = new SaleTicket();
        new Thread(st, "票贩子1").start();
        new Thread(st, "票贩子2").start();
        new Thread(st, "票贩子3").start();

    }

}
```

此时的程序将创建三个线程对象,并且这三个线程对象都将进行这10张票的销售.此时的程序在进行卖票处理的时候病没有任何问题(假象),下面可以模拟一下卖票中的延迟操作:

```javapackage 线程.线程的同步与死锁;

class SaleTicket implements Runnable {
    private int ticket = 10;
    @Override
    public void run() {
        while (true) {
            if(this.ticket > 0) {
                try {
                    Thread.sleep(100); // 模拟网络延迟
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "卖票, 还有票" + this.ticket --);
            }else{
                System.out.println("票已经卖光了!!");
                break;
            }
        }
    }
}

public class ThreadDemo1 {
    public static void main(String[] args) {
        SaleTicket st = new SaleTicket();
        new Thread(st, "票贩子1").start();
        new Thread(st, "票贩子2").start();
        new Thread(st, "票贩子3").start();

    }

}
```

> 这个时候追加了网络延迟问题就暴露出来了,而实际上这个问题一直都存在.

## 线程同步

经过分析以后已经可以确认同步问题所产生的主要原因了,那么下面就需要进行同步问题的解决,但是解决同步问题的关键是什么,是锁.指的是当某一个线程执行操作的时候,其他线程外面等待;

![](http://imgs.qipo.net/image/Thread/sync.jpg)

如果要想在程序之中实现这把锁的功能,就可以使用synchronized关键字来实现,利用此关键字可以定义我们的同步方法或同步代码快,在同步代码块的操作里面的代码只允许一个线程执行.

### 利用同步代码快进行处理

```
synchronized(同步对象) {
    同步代码操作
}
```

> 一般要进行同步对象处理的时候可以采用当前对象this进行同步.

范例: 利用同步代码快解决数据同步访问的问题

```java
package 线程.线程的同步与死锁;

class SaleThread implements Runnable {
    private int ticket = 100;
    @Override
    public void run() {
        while (true) {
            synchronized(this) { // 同步代码快
                if(this.ticket > 0) {
                    try {
                        Thread.sleep(100); // 模拟网络延迟
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println(Thread.currentThread().getName() + "卖票, 还有票" + this.ticket --);
                }else{
                    System.out.println("票已经卖光了!!");
                    break;
                }
            }
        }
    }
}

public class ThreadDemo2 {
    public static void main(String[] args) {
        SaleThread saleThread = new SaleThread();

        Thread threadA = new Thread(saleThread, "票贩子1");
        Thread threadB = new Thread(saleThread, "票贩子2");
        Thread threadC = new Thread(saleThread, "票贩子3");

        threadA.start();
        threadB.start();
        threadC.start();
    }
}
```

加入同步处理之后.程序的整体性能下降了,同步实际上会造成性能的降低.

### 利用同步方法解决

> 只需要在方法定义上使用synchronized关键字即可.

```java
package 线程.线程的同步与死锁;

class TicketThread implements Runnable {
    private int ticket = 100;
    public synchronized boolean sale() {
        if(this.ticket > 0) {
            try {
                Thread.sleep(100); // 模拟网络延迟
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + "卖票, 还有票" + this.ticket --);
            return true;
        }else{
            System.out.println("票已经卖光了!!");
            return false;
        }
    }
    @Override
    public void run() {
        while (this.sale()) {
            ;
        }
    }
}

public class ThreadDemo3 {
    public static void main(String[] args) {
        Runnable run = new TicketThread();
        new Thread(run, "票贩子1").start();
        new Thread(run, "票贩子2").start();
        new Thread(run, "票贩子3").start();
    }
}
```

在日后学习Java类库的时候会发现,系统中许多的类库使用的同步处理采用的都是同步方法.

## 死锁

> 死锁是进行多线程同步的处理之中有可能产生的一种问题,所谓的死锁指的是若干个线程彼此互相等待的状态;

下面通过一个简单的代码观察一下死锁的表现形式.

范例: 死锁的展示

> 现在死锁造成的主要原因是因为彼此都在互相等待着,等待着对方先让出资源.死锁实际上是一种开发之中出现的不确定的状态,有的时候代码如果处理不当则会不定期出现死锁,这是属于正常开发之中的调试问题.

> 若干个线程访问同意资源时,一定要进行同步处理,而过多的同步会造成死锁.

























