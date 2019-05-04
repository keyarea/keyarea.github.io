---
title: Android中活动（Activity）中的基本用法
date: 2019-05-04 20:47:38
tags:
  Java
  Android
category:
  Android
---

# 活动是什么

活动（Activity）是一种可以包含用户界面的组件，主要用于和用户进行交互。一个应用程序中可以包含零个或多个活动，但不包含任何活动的应用程序很少见。

# 活动的基本用法

## 活动的创建

项目中的任何活动都应该重写Activity中的Activity的onCreate()方法，如下：

```java
public class MainActivity extends AppCompatActivity {
        
    @Override
    protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
    }
}
```

可以看到，onCreate()方法非常简单，就是调用了父类的onCreate()方法。当然这只是默认的实现，后面我们还需要在里面加入很多自己的逻辑。

## 创建和加载布局

Android程序的设计讲究逻辑和视图分离，最好每一个活动都能对应一个布局，布局就是用来显示界面的内容的，因此我们来手动创建一个布局文件。

在res文件夹下创建或者打开layout文件夹，然后创建一个`first_layout.xml`的文件，在该文件中输入以下代码：

```xml

<!-- 
    LinearLayout:线性布局，可以视为html中的div，用于对于整个界面进行布局
    
    xmlns:andorid:指定的是xml文件的命名空间，不是对布局的主要设置，但是要有
    
    android:layout_width="match_parent"指的是当前的线性布局宽度占整个父元素，这里相对于
    当前的线性布局父元素为当前的窗体,所以宽度占满窗体
    
    android:layout_height="match_parent"指的是当前的线性布局高度占整个父元素，这里相对于
    当前的线性布局父元素为当前的窗体,所以高度占满窗体
   
    android:orientation="vertical":指的是当前控件为垂直摆放
    
    android:orientation 值:horizontal 元素水平摆放  |  vertical 元素垂直摆放
-->
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!--
        这里添加了一个Button元素，并在Button元素的内部增加了几个属性：
        android:id 这个是为当前元素定义了一个唯一标识符，之后可以在代码中对这个元素进行操作。
        @id+id/button_1 定义了一个id
        android:layout_width="match_parent" 表示当前元素的宽度和父元素一样宽。 
        android:layout_height="wrap_content" 表示当前元素的高度只能刚好包含里面的内容就行
        android:text="按钮" 指定了元素中显示的文字内容
    -->
    <Button
        android:id="@+id/button_1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="按钮"/>
</LinearLayout>
```

接下来要做的就是在活动中加载这个布局。

在Activity中的onCreate()方法中加入如下代码：

```java
public class FirstActivity extends AppCompatActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState){
    super.onCreate(savedInstanceState);
    // 这里调用了setContentView()方法来给当前的活动加载了一个布局，而在该方法中我们一般会传入一个布局文件的id，项目添加的任何资源都会在R文件中生成一个相应的资源id。只要调用`R.layout.first_layout`就会得到这个布局文件。
    setContentView(R.layout.first_layout);
  }
}
```

## 在AndroidMainifest文件中注册

所有的活动都要在`AndroidMainifest.xml`中进行注册才能生效。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <!--
          android:name=".FirstActivity" 用来指定具体注册的是哪一个活动
          这里的".FirstActivity"是"com.example.myapplication.FirstActivity"的缩写
          
          android:label 指定活动中标题栏的内容，标题栏是显示在活动最顶部的。
          需要注意的是，给主活动指定的label不仅会成为标题栏中的内容，还会成为启动器（Launcher）中应用程序显示的名称
        -->
        <activity android:name=".FirstActivity"
            android:label="MyApplication">
            
          <!-- 
            配置主活动    
          -->
          <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
          </intent-filter>
        </activity>
        
    </application>

</manifest>
```