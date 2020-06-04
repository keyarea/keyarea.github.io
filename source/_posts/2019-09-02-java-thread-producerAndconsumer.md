---
title: Java 线程 生产者与消费者模型
date: 2019-09-02 22:30:24
tags:
- Java
- Thread
category:
- Java
---

# Java多线程编程(综合案例)

在多线程的开发过程之中最为著名的案例就是生产者与消费者操作,该操作的主要流程如下:

- 生产者负责信息内容的生产;
- 每当生产者生产完一项完整的信息之后消费者要从者里面取走信息;
- 如果生产者没有生产则消费者需要等待它生产完成,如果消费者还没有对信息进行消费,则生产者应该等待消费处理完成后在继续生产.

## 程序的基本实现
 
可以将生产者与消费者定义为两个独立的线程类对象.

既然生产者与消费者是两个队里的线程,那么这两个独立的线程之间就需要有一个数据的保存的集中点,那么可以定义一个Message类进行数据的保存.

![](http://imgs.qipo.net/image/Thread/Producer&Comsumer.PNG)

范例: 实现程序的基本结构

```java
package 线程.生产者与消费者模型;

class Message {
    private String title;
    private String content;

    public void setContent(String content) {
        this.content = content;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getContent() {
        return content;
    }

    public String getTitle() {
        return title;
    }
}

class Producer implements Runnable {
    private Message msg;

    public Producer(Message msg) {
        this.msg = msg;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            if (x % 2 == 0) {
                this.msg.setTitle("王五");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                this.msg.setContent("王家第五子");
            } else {
                this.msg.setTitle("赵六");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                this.msg.setContent("赵家第六子");
            }
        }
    }
}

class Consumer implements Runnable {
    private Message msg;

    public Consumer (Message msg) {
        this.msg = msg;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(this.msg.getTitle() + "-" + this.msg.getContent() );
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        Message msg = new Message();
        new Thread(new Producer(msg)).start();
        new Thread(new Consumer(msg)).start();
    }
}
```

> 通过整个代码的执行你会发现有两个主要问题:

- 数据不同步;
- 生产一个取走一个,但是发现有重复生产和重复取出的问题;

## 解决问题

### 数据同步

如果要解决问题,首先解决的就是数据同步的处理问题,如果要想解决数据同步最简单的做法是使用synchronized关键字定义同步代码块或者同步方法,于是这个时候对于同步的处理就可以直接在Message类中完成.

范例:解决数据同步问题

```java
package 线程.生产者与消费者模型;

class Message {
    private String title;
    private String content;

    public synchronized void set (String title, String content) {
        this.title = title;
        this.content = content;
    }

    public synchronized String get () throws InterruptedException {
        Thread.sleep(10);
        return this.title + " -  " + this.content;
    }

}

class Producer implements Runnable {
    private Message msg;

    public Producer(Message msg) {
        this.msg = msg;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            if (x % 2 == 0) {
                msg.set("王五", "王家第五子");
            } else {
                msg.set("赵六", "赵家第六字");
            }
        }
    }
}

class Consumer implements Runnable {
    private Message msg;

    public Consumer (Message msg) {
        this.msg = msg;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            try {
                System.out.println(msg.get());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        Message msg = new Message();
        new Thread(new Producer(msg)).start();
        new Thread(new Consumer(msg)).start();
    }
}
```

> 在进行同步处理的时候肯定需要一个同步的处理对象,那么此时肯定要将同步操作交与Message类处理是最合适的.

> 这个时候发现数据已经可以正常的保持一致了,但是对于重复操作的问题依然存在.

### 线程等待与唤醒

> 如果说现在要解决生产者与消费者的问题,那么最好的解决方案就是使用等待与唤醒机制.而对于等待与
唤醒操作机制,主要是依靠Object类中提供的方法处理的;

> 等待机制:
> - 死等: `public final void wait() throws InterruptedException;`
> - 设置等待时间: `public final void wait(long timeout) throws InterruptedException;`
> - 设置等待时间: `public final void wait(long timeout, int nanos) throws InterruptedException;`

> 唤醒机制:
> - 唤醒一个等待线程: `public final void notify();`
> - 唤醒全部等待线程: `public final void notifyAll()`

如果此时有若干个等待线程的话,那么notify()表示的是唤醒第一个等待的,而其他的线程继续等待,而notifyAll()会唤醒
所有等待的线程,那个线程的优先级高就可能先执行.

对于当前的问题主要的解决应该是通过Message类完成处理.

范例: 修改Message类

```java
package 线程.生产者与消费者模型;

class Message {
    private String title;
    private String content;
    private boolean flag = true;
    // flag 为true,代表着允许生产,不允许消费
    // flag 为false,代表着允许消费,不允许生产
    public synchronized void set (String title, String content) {
        if (!this.flag) { // 还未消费,等待消费
            try {
                super.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        this.title = title;
        this.content = content;
        this.flag = false; // 生产完成了
        this.notify(); // 唤醒等待的线程
    }

    public synchronized String get () {
        if (this.flag) { // 还未生产,等待生产
            try {
                super.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        try {
            Thread.sleep(10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        try {
            return this.title + " -  " + this.content;
        }finally {
            this.flag = true; // 继续生产
            super.notify(); // 唤醒等待的线程
        }
    }

}

class Producer implements Runnable {
    private Message msg;

    public Producer(Message msg) {
        this.msg = msg;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            if (x % 2 == 0) {
                msg.set("王五", "王家第五子");
            } else {
                msg.set("赵六", "赵家第六字");
            }
        }
    }
}

class Consumer implements Runnable {
    private Message msg;

    public Consumer (Message msg) {
        this.msg = msg;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            System.out.println(msg.get());
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        Message msg = new Message();
        new Thread(new Producer(msg)).start();
        new Thread(new Consumer(msg)).start();
    }
}
```

> 这种处理形式就是在进程多线程开发过程之中最原始的处理方案,整个的等待,同步,唤醒机制都由开发者自行通过原生代码实现控制.

