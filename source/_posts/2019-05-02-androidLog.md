---
title: Android的日志工具Log
date: 2019-05-02 14:44:27
tags:
  Java
  Android
categories:
  Android
---

这里说一下Android中的日志工具的使用方法，这对以后的Android开发会有很大的帮助。

# 使用Android的日志工具Log

Android中的日志工具类是Log（android.util.Log）,这个类中提供了如下5个方法来供我们打印日志。

- Log.v():用于打印那些最为琐碎的，意义最小的日志信息。对应级别verbose，是android日志类面级别最低的一种。
- Log.d():用于打印一些调试信息，这些信息对你调试程序和分析问题应该帮助的。对应级别debug，比verbose高一级。
- Log.i(): 用于打印一些比较重要的数据，这些数据应该是你非常想看到的，可以帮你分析用户行为数据。对应级别info，比debug高一级。
- Log.w(): 用于打印一些警告信息。提示程序在这个地方可能会有潜在风险，最好去修复这些出现警告的地方。对应级别warn，比info高一级。
- Log.e(): 用于打印程序中的错误信息。比如程序进入到了catch语句当中。当有错误信息打印出来的时候，一般代表你的程序出现了严重问题了，必须尽快修复。对应级别error，比wran高一级。

实例：打开Activity，在onCreate()方法中添加一行打印日志的语句

```java
protected void onCreate(Bundle saveInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.hello_world_layout);

    // 该方法传入两个参数：
    // 第一个参数是tag，一般传入当前的类名，主要用于对打印信息进行过滤；
    // 第二个参数是msg，即想要打印的具体的内容
    Log.d("HelloWorldActivity", "onCreate execute");
}
```

# 为什么使用Log而不使用System.out

`System.out.println();`：该方法缺点很多，日志打印不可控制、打印时间无法确定、不能添加过滤器、日志没有级别区分......

Log把上面的所有缺点全部改好了吗？虽然谈不上全部，但是Log已经做的相当不错了。

在Android Studio当中，比如你想打印一条debug级别的日志，那么只需要输入`logd`,然后按下Tab键，结汇帮你自动补全一条完整的打印语句。

- logd：debug级别的打印日志；
- logi: info级别的打印日志；
- logw：warn级别的打印日志；
- loge： error级别的打印日志。

生成的完整的打印日志：

```java
Log.d(TAG, "onCreate: ");
```

> 由于Log所有的打印方法都要求传入一个tag参数，每次输入显然太过麻烦。    
> 这里有一个小技巧，我们在onCreate()方法的外面输入`logt`,然后按下Tab键，这时就会以当前的类名作为值自动生成一个TAG常量，如下所示：

```java

public class MainActivity extends AppCompatActivity {
    private static final String TAG = "MainActivity";
}
```

