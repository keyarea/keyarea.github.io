---
title: Java 多线程相关案例
date: 2019-09-02 22:34:32
tags:
- Java
- Thread
category:
- Java
---

# 多线程相关案例

## 多线程案例1:加减操作

> 设计4个线程对象,两个线程执行减操作,两个线程执行加操作

```java
package 线程.多线程相关案例;

class Resource {
    private int num = 0;
    private boolean flag = true;
    // flag为true可以执行加法操作,不可以执行减法操作
    // flag为false可以执行减法操作,不可以执行加法操作
    public synchronized void add() throws Exception {
       while (!this.flag) { // flag为false只能执行减法操作
           super.wait(); // 需要等待
       }
       Thread.sleep(100);
        this.num++;
        System.out.println(Thread.currentThread().getName() + "运行, num = " + this.num);
        this.flag = false; // 可以进行减法操作
        super.notifyAll(); // 唤醒其他线程
    }
    public synchronized void sub() throws Exception {
        while(this.flag) { // flag为true只能进行加法操作,减法操作需要等待
            super.wait();
        }
        Thread.sleep(100);
        this.num--;
        System.out.println(Thread.currentThread().getName() + "运行, num = " + this.num);
        this.flag = true    ; // 可以进行加法操作
        super.notifyAll(); // 唤醒其他线程
    }
}

class AddThread implements Runnable {
    private Resource res;

    public AddThread(Resource res) {
        this.res = res;
    }
    @Override
    public void run() {
        for (int i = 0 ; i < 50 ; i++) {
            try {
                this.res.add();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

class SubThread implements Runnable {
    private Resource res;

    public SubThread(Resource res) {
        this.res = res;
    }

    @Override
    public void run() {
        for (int i = 0 ; i < 50 ; i++) {
            try {
                this.res.sub();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        Resource res = new Resource();
        AddThread at = new AddThread(res);
        SubThread st = new SubThread(res);

        new Thread(at, "加法操作-1").start();
        new Thread(at, "加法操作-2").start();

        new Thread(st, "减法操作-1").start();
        new Thread(st, "减法操作-2").start();
    }
}
```

## 多线程案例分析

设计一个生产电脑和搬运电脑的类,要求生产出一台电脑就搬走一台电脑,如果没有新的电脑生产出来,则搬运工要等待新电脑生产出来;如果产出的电脑没有搬走,则要等待电脑搬走之后再生产,并统计出生产的电脑数量.


在本程序之中实现的就是一个标准的生产者与消费者的处理模型,那么下面实现具体的程序代码:

```java
package 线程.多线程相关案例;

import java.util.function.Predicate;

class Computer {
   private static int count = 0;
   private String name;
   private double price;

   public Computer(String name,double price) {
       this.name = name;
       this.price = price;
       count++;
   }

    @Override
    public String toString() {
       return "第" + count + "台电脑,电脑名字: " + this.name + ", 电脑价格: " + this.price;
    }
}

class Asset {
    private Computer computer;

    public synchronized void make() throws Exception{
        while (this.computer != null) {
            super.wait();
        }
        this.computer = new Computer("浪潮", 1200.00);
        System.out.println("生产电脑:" + this.computer);
        super.notifyAll();
    }

    public synchronized void get() throws Exception {
        while (this.computer == null) {
           super.wait();
        }
        System.out.println("搬运电脑:" + this.computer);
        this.computer = null;
        super.notifyAll();
    }
}

class Producer implements Runnable{
    private Asset asset;

    public Producer(Asset asset) {
        this.asset = asset;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 50 ; x++) {
            try {
                this.asset.make();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

class Consumer implements Runnable{
    private Asset asset;

    public Consumer(Asset asset) {
        this.asset = asset;
    }

    @Override
    public void run() {
        for (int x = 0 ; x < 100 ; x ++) {
            try {
                this.asset.get();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}


public class ThreadDemo1 {
    public static void main(String[] args) {
        Asset asset = new Asset();
        Producer pt = new Producer(asset);
        Consumer ct = new Consumer(asset);

        new Thread(pt, "生产电脑-1").start();
        new Thread(pt, "生产电脑-2").start();
        new Thread(ct, "搬运电脑-1").start();
        new Thread(ct, "搬运电脑-2").start();
    }
}
```

## 多线程案例分析3 实现一个竞拍抢答程序
 
> 要求设置三个抢答者(三个线程),而后同时发出抢答指令,抢答成功给出成功提示,未抢答成功者给出失败提示.

由于这个多线程的操作由于里面需要牵扯到数据的返回问题,那么现在最好使用的Callable接口是比较方便的一种操作形式

```java
package 线程.多线程相关案例;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

class MyThread implements Callable<String> {
    private boolean flag = false;
    @Override
    public String call() throws Exception {
        synchronized (this) {
            if (!this.flag) {
                this.flag = true;
                return Thread.currentThread().getName() + "抢答成功!";
            }else{
                return Thread.currentThread().getName() + "抢答失败!";
            }
        }
    }
}

public class ThreadDemo2 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        MyThread myThread = new MyThread();
        FutureTask<String> task1 = new FutureTask<>(myThread);
        FutureTask<String> task2 = new FutureTask<>(myThread);
        FutureTask<String> task3 = new FutureTask<>(myThread);

        new Thread(task1, "竞赛者A").start();
        new Thread(task2, "竞赛者B").start();
        new Thread(task3, "竞赛者C").start();

        System.out.println(task1.get());
        System.out.println(task2.get());
        System.out.println(task3.get());

    }
}
```

