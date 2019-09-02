---
title: Java 线程常用操作方法
date: 2019-09-02 22:23:21
tags:
- Java
- Thread
category:
- Java
---

# 线程常用操作方法

多线程的主要操作方法都在Thread类中定义了.

## 线程的命名与取得

多线程的运行状态是不确定的,那么在程序的开发之宗为了可以获取到一些需要使用到线程就能够依靠线程的名字来进行操作.所以线程的名字是一个至关重要的概念,这样在Thread类之中就提供有线程名称的处理.

> 构造方法: `public Thread(Runnable target, String name);`

> 设置名字: `public final void setName(String name);`

> 取得名字: `public final String getName();`

对于线程对象的获得是不可能只是依靠一个this来完成的,因为线程的状态不可控,但是有一点是明确的,所有的线程对象一定要执行run()方法,那么这个时候可以考虑获取当前线程,在Thread类里面提供有获取当前
线程的方法:

> 获取当前线程: `public static Thread currentThread();`

范例: 观察线程的命名操作

```java
package 线程.线程常用操作方法;


class MyThread implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        Runnable runnable = new MyThread();
        new Thread(runnable, "线程A").start();
        new Thread(runnable).start();
        new Thread(runnable, "线程B").start();
    }
}
```

当开发者为线程设置名字的时候就使用设置的名字,而如果没有设置名字,则会自动生成一个不重复的名字,这种自动的属性命名主要是依靠了static属性完成的,在Thread类里面定义有如下操作:

```
pirvate static int threadInitNumber;
private static synchronized int nextThreadNum() {
  return threadInitNumber++;
}
```

范例: 观察下面的代码

```java
package 线程.线程常用操作方法;


class MyThread implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        MyThread mt = new MyThread();
        new Thread(mt, "线程A").start();
        mt.run();
    }
}
```

> 通过此时的代码可以发现当使用了`mt.run()`直接在住方法调用线程类对象啊中的run()方法所获得的线程对象的名字为`main`,所以可以得到一个结论: 住方法也是一个线程.
那么现在问题来了,所有的线程都是在进程上的划分,那么进程在哪里?
>> 每当使用java命令执行程序的时候就表示启动了一个JVM的进程,一台电脑上可以启动若干个JVM进程,所以每一个JVM的进程都会有各自的线程.

> 在任何的开发之中,主线程可以创建若干个子线程,创建子线程的目的是可以将一些复杂逻辑或者比较耗时的逻辑交由子线程处理;

范例: 子线程处理

```java
package 线程.线程常用操作方法;

public class ThreadDemo1 {
    public static void main(String[] args) {
        System.out.println("任务一");
        System.out.println("任务二");
//        int temp = 0;
//        for(int i = 0; i< Integer.MAX_VALUE; i++) {
//            temp+=i;
//        }
        new Thread(() -> {
            int temp = 0;
            for(int i = 0; i< Integer.MAX_VALUE; i++) {
                temp+=i;
            }
        }).start();
        System.out.println("任务N");
    }
}
```

> 子线程负责处理整体流程,而子线程负责处理耗时操作.

## 线程休眠

>如果说现在希望某一个线程可以暂缓执行一次,那么就可以使用休眠处理,在Thread类中,定义的休眠的方法如下:
>> - 休眠: `public static void sleep(long millis) throws InterruptedException;`
>> - 休眠: `public static void sleep(long millis, int nanos) throws InterruptedException;`
> 在休眠的时候有可能会产生中断异常`InterruptedException`,中断异常属于Exception的子类,所以证明该异常必须进行处理.

范例: 观察休眠处理

```java
package 线程.线程常用操作方法;

public class ThreadSleepDemo {
    public static void main(String[] args) {
        new Thread(()-> {
            for (int x = 0; x < 10; x++) {
                System.out.println(Thread.currentThread().getName() + ", x = " + x);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "线程A").start();
    }
}
```

> 休眠的主要特点就是可以自动实现线程的唤醒,以继续进行后续的处理.但是需要注意的是,如果现在有多个线程对象,那么休眠也是有先后顺序的.

范例: 产生多个线程对象进行休眠处理

```java
package 线程.线程常用操作方法;

/**
 * 多个线程休眠
 */
public class MultiThreadSleepDemo {
    public static void main(String[] args) {
        for (int x = 0; x < 5; x++) {
            Runnable run = ()-> {
                for(int y = 0; y < 10; y++){
                    System.out.println(Thread.currentThread().getName() + ", y = " + y);
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            };
            new Thread(run, "线程-"+ x).start();
        }
    }
}
```

此时将产生五个线程对象,并且者五个线程对象执行的方法体是相同的.此时从程序执行感觉上来讲好像是若干个线程一起进行了休眠,而后一起进行了唤醒,但是实际上是有差别的.

## 线程中断

> 在之前发现线程的休眠里面提供有一个中断异常,实际上就证明线程的休眠是可以被打断的,而打断肯定是由其他线程完成的,在Thread类里面提供有这种中断执行的处理方法:
>> - 判断线程是否被中断: `public boolean isInterrupt();`
>> - 中断线程执行: `public void interrupt();`

范例: 观察线程的中断处理操作

```java
package 线程.线程常用操作方法;

public class ThreadInterruptDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(()-> {
            System.out.println("午休一下!!");
            try {
                Thread.sleep(100000);
                System.out.println("2点到了,该上班了!!");
            } catch (InterruptedException e) {
                System.out.println("打扰老子睡觉,老子宰了你!");
            }
        });
        thread.start();
        Thread.sleep(1000);
        
        if(!thread.isInterrupted()) {
            System.out.println("打扰一下,有点急事.");
            thread.interrupt();
        }
    }
}
```

> 所有正在执行的线程都是可以被打断的,中断线程必须进行异常的处理.

## 线程的强制执行

> 所谓的线程的强制执行指的是当满足某些条件之后,某一个线程对象将一直独占资源,一直到该线程的程序执行结束.

范例: 观察一个没有强制执行的程序

```java
package 线程.线程常用操作方法;

public class ThreadStart {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(()-> {
            for(int x = 0; x < 100; x++ ) {
                System.out.println(Thread.currentThread().getName() + "线程运行, x = " + x);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "线程对象");
        thread.start();
        for (int y = 0; y < 100; y++) {
            System.out.println("main线程运行, y=" + y );
            Thread.sleep(100);
        }
    }
}
```

> 这个时候,主线程和子线程都在交替执行着,但是如果说现在你希望主线程独占执行.那么就可以使用Thread类中的方法:
>> - 强制执行: `public final void join() throws InterruptException;`

```java
package 线程.线程常用操作方法;

public class ThreadStart {
    public static void main(String[] args) throws InterruptedException {
        Thread mainThread = Thread.currentThread();
        Thread thread = new Thread(()-> {
            for(int x = 0; x < 100; x++ ) {
                if(x == 4) {
                    try {
                        mainThread.join(); 
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                System.out.println(Thread.currentThread().getName() + "线程运行, x = " + x);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }, "线程对象");
        thread.start();
        for (int y = 0; y < 100; y++) {
            System.out.println("main线程运行, y=" + y );
            Thread.sleep(100);
        }
    }
}
```

> 在进行线程强制执行的时候一定要获取强制执行线程的对象之后才可以执行join()调用.
 

## 线程的礼让

> 线程的礼让指的是先将资源让出去让别的线程先执行.线程的礼让可以使用Thread中提供的方法:
>> - 礼让: `public static void yeild();`

范例: 使用礼让操作

```java
package 线程.线程常用操作方法;

/**
 * 线程礼让
 */

public class ThreadYield {
    public static void main(String[] args) throws Exception{
        Thread mainThread = Thread.currentThread();
        Thread th = new Thread(()-> {
            for (int x = 0; x < 100; x++) {

                if(x % 3 == 0) {
                    Thread.yield();
                    System.out.println("子线程礼让");
                }
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                System.out.println(Thread.currentThread().getName() + "线程运行了, x = " + x);
            }
        });
        th.start();

        for (int y = 0; y < 100; y++) {
            System.out.println("main线程运行, y=" + y );
            Thread.sleep(100);
        }
    }
}
```

礼让执行的时候每一次调用`yield()`方法都只会礼让一次当前的资源.

## 线程优先级

从理论上来讲,线程的优先级越高越有可能先执行(越有可能先抢占到资源).在Thread类里面针对
优先级有两个处理方法:

- 设置优先级: `public final void setPriority(int newPriority)`;
- 获取优先级: `public final int getPriority()`;

在进行优先级定义的时候都是通过int型的数字来完成的,而对于此数字的选择在Thread类里面就定义有三个常量:

- 最高优先级: `public static final int MAX_PRIORITY`, 10;
- 中等优先级: `public static final int NORM_PRIORITY`, 5;
- 最低优先级: `public static final int MIN_PRIORITY`, 1;

范例: 观察优先级

```java
package 线程.线程常用操作方法;

/**
 * 线程优先级
 */

public class ThreadPriority {
    public static void main(String[] args) {
        Runnable run = () -> {
            for (int x = 0; x < 10; x++) {
                System.out.println(Thread.currentThread().getName()+ "执行");
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };
        Thread threadA = new Thread(run, "线程A");
        Thread threadB = new Thread(run, "线程B");
        Thread threadC = new Thread(run, "线程C");
        threadA.setPriority(Thread.MAX_PRIORITY);
        threadB.setPriority(Thread.NORM_PRIORITY);
        threadC.setPriority(Thread.MIN_PRIORITY);
        
        threadA.start();
        threadB.start();
        threadC.start();

    }
}

```

主线程也是一个线程,那么主线程的优先级为多少呢?

```java
package 线程.线程常用操作方法;

/**
 * 线程优先级
 */

public class ThreadPriority {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getPriority()); // 5
    }
}
```

主线程属于中等优先级,而默认创建的线程也是中等优先级.






