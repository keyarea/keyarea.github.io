---
title: Java开发常用类库
date: 2019-09-09 11:10:04
tags:
- Java
category:
- Java
---


## UUID类

> UUID是一种生成无重复字符串的程序类,这种程序类的主要功能是根据时间戳实现一个自动的无重复的字符串定义.

> 一般在获取UUID的时候往往都是随机生成一个内容,所以可以通过如下方法获取:
> - 获取UUID对象: `public static UUID randomUUID()`;
> - 根据字符串获取UUID内容: `public static UUID fromString(String name)`;

```java
import java.util.UUID;

public class UUIDTest {
    public static void main(String[] args) {
        UUID uuid = UUID.randomUUID();
        System.out.println(uuid.toString());
    }
}
```

> 在对一些文件进行自动命名处理的情况下,UUID类型非常好用.

## Optional类

> Optional类主要是进行null的相关处理,在以前进行程序开发的时候,如果为了程序之中出现空指向,往往可以追加有null的验证.

范例:传统的引用传递问题

```java
public class OptionalTest {
    public static void main(String[] args) {
        print(null);
    }

    private static void print(String str){
        if(str != null){
            System.out.println(str.toString());
        }
    }
}
```

> 在引用接收的一方往往都是被动判断,所以为了解决这种被动的处理操作,在Java中提供有一个Optional类,这个类可以实现null的处理操作,在这个类里面提供有如下的操作方法:
> - 返回空数据: `public static <T> Optional<T> empty()`;
> - 获取数据: `public T get()`;
> - 保存数据,但是不允许出现null: `public static <T> Optional<T> of(T value)`;
>  - 如果在保存数据的时候存在有null,则会抛出NullPointerException异常
> - 保存数据,允许为null: `public static <T> Optional<T> ofNullable(T able)`;
> - 空的时候返回其它数据: `public T orElse(T other)`;

![](http://imgs.loong.io/20190908101310_wCY5wM_Screenshot.jpeg)

范例: 修改程序,按照正规的结构完成

```java
import java.util.Optional;

interface IMessage{
    public String getContent();
}

class MessageUtil {
    private MessageUtil(){}

    public static Optional<IMessage> getMessage() {
        return Optional.of(new MessageImpl());
    }

    public static void useMessage(IMessage msg) {
        System.out.println(msg.getContent());
    }
}

class MessageImpl implements IMessage{
    @Override
    public String getContent() {
        return "www.qipo.net";
    }
}

public class OptionalTest1 {
    public static void main(String[] args) {
        IMessage temp = MessageUtil.getMessage().get();
        MessageUtil.useMessage(temp);
    }
}
```

由于Optional类中允许保存有null的内容,所以在数据获取的时候也可以进行null的处理.但是如果为null,则在使用get()获取数据的时候就会出现`Exception in thread "main" java.util.NoSuchElementException: No value present`的异常.
所以此时可以更换为ofElse()方法.

范例: 处理null的值

```java
import java.util.Optional;

interface IMessage{
    public String getContent();
}

class MessageUtil {
    private MessageUtil(){}

    public static Optional<IMessage> getMessage() {
        return Optional.ofNullable(null);
    }

    public static void useMessage(IMessage msg) {
        System.out.println(msg.getContent());
    }
}

class MessageImpl implements IMessage{
    @Override
    public String getContent() {
        return "www.qipo.net";
    }
}

public class OptionalTest1 {
    public static void main(String[] args) {
        IMessage temp = MessageUtil.getMessage().orElse(new MessageImpl());
        MessageUtil.useMessage(temp);
    }
}
```

在所有引用数据类型的操作处理之中,null是一个重要的技术问题,所以JDK1.8之后对于null的处理很有帮助,同时也是日后进行项目开发之中使用次数最多的一个程序类. 

## ThreadLocal类

在真正的去了解ThreadLocal类作用的时候编写一个简单的程序做一个先期分析 :

```java
class Message {
    private String meg;

    public String getMeg() {
        return meg;
    }

    public void setMeg(String meg) {
        this.meg = meg;
    }
}

class Channel {
    private static Message msg;
    private Channel(){}
    public static void setMessage(Message m) {
        msg = m;
    }

    public static void send() {
        System.out.println("[发送消息]" + msg.getMeg());
    }
}

public class ThreadLocalTest {
    public static void main(String[] args) {
        Message msg = new Message();
        msg.setMeg("消息");
        Channel.setMessage(msg);
        Channel.send();
    }
}
```

![](http://imgs.loong.io/20190908105406_Oh17FM_Screenshot.jpeg)

对于当前的程序实际上采用的是一种单线程的模式来进行处理的,那么再多线程情况下能否实现完全一致的操作效果呢?为此我们启动三个线程进行处理.

范例: 多线程的影响

```java
class Message {
    private String meg;

    public String getMeg() {
        return meg;
    }

    public void setMeg(String meg) {
        this.meg = meg;
    }
}

class Channel {
    private static Message msg;
    private Channel(){}
    public static void setMessage(Message m) {
        msg = m;
    }

    public static void send() {
        System.out.println("[发送消息]" + Thread.currentThread().getName() + "-" + msg.getMeg());
    }
}

public class ThreadLocalTest {
    public static void main(String[] args) {
        new Thread(() -> {
            Message msg = new Message();
            msg.setMeg("线程A的消息");
            Channel.setMessage(msg);
            Channel.send();
        }, "线程A").start();
        new Thread(() -> {
            Message msg = new Message();
            msg.setMeg("线程B的消息");
            Channel.setMessage(msg);
            Channel.send();
        }, "线程B").start();
        new Thread(() -> {
            Message msg = new Message();
            msg.setMeg("线程C的消息");
            Channel.setMessage(msg);
            Channel.send();
        }, "线程C").start();

    }
}
```

这个时候消息的处理产生了影响.

![](http://imgs.loong.io/20190908112149_vwSI5h_Screenshot.jpeg)

在保持Channel(所有发送的通道)核心结构不改变的情况下,需要考虑到每个线程的独立操作问题.那么在这样的情况下就发现对于Channel类而言,除了要保留有发送的消息之外,还应该多存放有一个每一个线程的标记(当前线程),
那么这个时候我们就可以通过ThreadLocal类来存放数据.在ThreadLocal类里面提供有如下的操作方法:

- 构造方法: `public ThreadLocal()`;
- 设置数据: `public void set(T value)`;
- 取出数据: `public T get()`;
- 删除数据: `public void remove()`;

![](http://imgs.loong.io/20190908113324_LYlF82_Screenshot.jpeg)

范例: 解决线程同步问题

```java
class Message {
    private String meg;

    public String getMeg() {
        return meg;
    }

    public void setMeg(String meg) {
        this.meg = meg;
    }
}

class Channel {
    private static final ThreadLocal<Message> THREAD_LOCAL = new ThreadLocal<>();
    private Channel(){}
    public static void setMessage(Message m) {
        THREAD_LOCAL.set(m);
    }

    public static void send() {
        System.out.println("[发送消息]" + Thread.currentThread().getName() + "-" + THREAD_LOCAL.get().getMeg());
    }
}

public class ThreadLocalTest {
    public static void main(String[] args) {
        new Thread(() -> {
            Message msg = new Message();
            msg.setMeg("线程A的消息");
            Channel.setMessage(msg);
            Channel.send();
        }, "线程A").start();
        new Thread(() -> {
            Message msg = new Message();
            msg.setMeg("线程B的消息");
            Channel.setMessage(msg);
            Channel.send();
        }, "线程B").start();
        new Thread(() -> {
            Message msg = new Message();
            msg.setMeg("线程C的消息");
            Channel.setMessage(msg);
            Channel.send();
        }, "线程C").start();

    }
}
```

每个线程通过ThreadLocal只允许保存一个数据.


## 定时器

定时器的主要操作是进行定时任务的处理.在Java中提供有定时任务的支持,但是这种任务的处理只是实现了一种间隔触发的操作.

如果要想实现定时的处理操作主要是需要有一个定时操作的主体类,以及一个定时任务的控制.可以使用两个类实现:

- `java.util.TimerTask`类:实现定时任务处理;
- `java.util.Timer`类:进行任务的处理,启动的方法:
 - 任务启动: `public void schedule(TimerTask task, long delay)`,延迟单位为毫秒;
 - 间隔触发: `public void scheduleAtFixedRate(TimerTask task, long delay, long period)`;
 
范例: 实现定时任务处理

```java
import java.util.Timer;
import java.util.TimerTask;

class MyTask extends TimerTask {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "定时任务启动, 当前时间为: " + System.currentTimeMillis());
    }
}

public class TimerTest {
    public static void main(String[] args) {
        Timer timer = new Timer(); // 定时任务
        // 定义间隔任务,1000毫秒之后开始执行,1000毫秒执行一次
        timer.scheduleAtFixedRate(new MyTask(), 1000, 1000);
//        timer.schedule(new MyTask(), 1000); // 1000毫秒之后开始执行
    }
}

```

![](http://imgs.loong.io/20190908115725_Wqt9ar_Screenshot.jpeg)


这种定时是由JDK最原始的方式提供的支持,但是实际上开发之中利用此类方式进行的定时处理实现的代码会非常复杂.

## Base64加密工具

正常来说,加密基本上永远都伴随着解密,所谓的加密或者是解密往往都需要有一些所谓的规则.在JDK1.8开始提供有一个新的加密处理操作类,Base64处理,在这个类里面有两个内部类:

- Base64.Encoder:进行加密处理;
  - 加密处理: `public byte[] encode(byte[] src)`
- Base64.Decoder:进行解密处理;
  - 解密处理: `public byte[] decode(String src)`
 
范例: 加密与解密操作

```java
import java.util.Base64;

public class Base64Test {
    public static void main(String[] args) {
        String str = "www.qipo.net"; // 要进行加密的内容
        
        // 加密操作
        String enStr = new String(Base64.getEncoder().encode(str.getBytes()));
        System.out.println(enStr); // 加密之后的字符串

        // 解密操作
        String deStr = new String(Base64.getDecoder().decode(enStr));
        System.out.println(deStr); // 解密之后的字符串
    }
}
```

虽然Base64可以实现加密与解密操作的处理,但是由于是一个公共的算法,所以如果直接对数据进行加密往往并不安全,最好的做法是使用盐值操作.

范例: 盐值

```java

import java.util.Base64;

public class Base64Test1 {
    public static void main(String[] args) {
        String salt = "jfidsj";

        String str = "www.qipo.net" + salt; // 要进行加密的内容

        // 加密操作
        String enStr = new String(Base64.getEncoder().encode(str.getBytes()));
        System.out.println(enStr); // 加密之后的字符串

        // 解密操作
        String deStr = new String(Base64.getDecoder().decode(enStr));
        System.out.println(deStr); // 解密之后的字符串
    }
}
```

即使现在有盐值,实际上加密效果也不是很好,最好的办法就是多次加密。

范例：复杂的加密操作

```java
import java.util.Base64;

class StringUtil{
    private static final int REPEAT = 5; // 加密次数
    private static final String SALT = "qipo"; // 盐值
    private StringUtil(){}

    /**
     * 加密处理
     * @param str 需要加密的字符串
     * @return 加密后的数据
     */
    public static String encode(String str) { // 加密处理
        String temp =   str + "{" + SALT + "}"; // 盐值不对外公布
        byte[] data = temp.getBytes();
        for (int i = 0; i < REPEAT; i++) {
            data = Base64.getEncoder().encode(data);
        }
        return new String(data);
    }

    /**
     * 解密处理
     * @param str 需要进行解密的字符串
     * @return 解密之后的数据
     */
    public static String decode(String str) {
        byte[] data = str.getBytes();
        for (int i = 0; i < REPEAT ; i++) {
            data = Base64.getDecoder().decode(data);
        }
        return new String(data).replaceAll("\\{\\w+\\}", "");
    }
}

public class Base64Test2 {
    public static void main(String[] args) {
        String temp = "www.qipo.net";

        String encodeStr = StringUtil.encode(temp);
        System.out.println(encodeStr);
        String decodeStr = StringUtil.decode(encodeStr);
        System.out.println(decodeStr);
    }
}
```

最好的做法是使用2-3种加密程序，同时再找到一些完全不可解密的加密算法。
