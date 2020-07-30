---
title: Android常用控件
date: 2020-07-30 08:21:38
tags:
  Android Java
category:
  Android
---

## 常用控件的使用方法

### TextView

> TextView是Android中最简单的一个空间，主要用于在界面上显示一段文本信息。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <TextView
        android:id="@+id/text_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="Hello World!"
        android:textSize="24sp"
        android:textColor="#00ff00"/>

  </LinearLayout>
```

- `android:id`:给当前控件一定了一个唯一标识符;
- `android:text`: 指定了TextView中显示的文本内容;
- `android:layout_height`和`android:layout_width`: 分别指定了空间的高度和宽度;其中可选则属性值为:
  - `match_parent`和`fill_parent`:表示让当前的控件的大小和父布局的大小一样，也就是由父布局来决定当前控件的大小;
  - `wrap_content`: 标识让当前控件大小能够刚好包住里面的内容，也就是由控件的内容决定当前控件的大小;
- `android:gravity`: 来指定文字的对齐方式。可选值有`top`,`bottom`,`left`,`right`,`center`等，可以用`|`来同时指定多个值，这里我们指定的`center`，效果等同于`center_vertical|center_horizontal`,表示文字在垂直和水平方向都居中对齐。
- `android:textSize`:属性可以指定文字的大小,在Android中文字大小使用`sp`作为单位。
- `android:textColor`:属性可以指定文字的颜色。
- 还有很多属性...

### Button

> Button是程序用于和用户交互的一个重要控件;

```xml
    <Button
        android:id="@+id/button1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button" />
```

> 需要注意的是，我们在布局文件里设置的文字是`Button`，但最终显示的结果却是`BUTTON`。这是由于系统会对Button中的所有英文字母自动进行大写转换 ，可以使用如下配置禁用这一默认特性:
>
> `android:textAllCaps="false"`

Button一般用于与用户交互，需要监听用户的点击事件，并做出相应的响应。为Button的点击事件注册一个监听器：

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);
        // 使用匿名类的方式来注册监听器
        //button1.setOnClickListener(new View.OnClickListener(){
        //    @Override
        //    public void onClick(View v) {
        //        Toast.makeText(MainActivity.this, "clicked Button", Toast.LENGTH_SHORT).show();
        //    }
        //});
        // 使用lambda表达式来注册监听器
        button1.setOnClickListener((v) -> {
            Toast.makeText(MainActivity.this, "click Button", Toast.LENGTH_SHORT).show();
        });
    }
}
```

以上是使用匿名类以及lambda表达式的形式进行注册监听器，还可以使用实现接口的方式进行,如下：

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);
        // 使用实现接口的形式来注册监听器
        button1.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                Toast.makeText(MainActivity.this, "clicked Button", Toast.LENGTH_SHORT).show();
                break;
            default:
                break;
        }
    }
}
```

### EditText

> EditText是程序用于和用户进行交互的另一个重要控件，它允许用户在控件中输入和编辑内容，并可以在程序中对这些内容进行处理。

```xml
    <EditText
        android:id="@+id/editText1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入一些内容"
        android:maxLines="2"/>
```

- `android:hint`:属性指定了一段提示性文本。
- `android:maxLines`:属性指定了EditText的最大行数为两行，这样当输入的内容超过两行时，文本就会向上滚动，而EditText则不会再继续拉伸。

我们还可以利用EditText做一些与用户交互的功能，比如通过点击按钮获得EditText中输入的内容。如下：

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private EditText editText;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);

        // 获取editText
        editText = findViewById(R.id.editText1);
        button1.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                String str = editText.getText().toString();
                Toast.makeText(MainActivity.this, str, Toast.LENGTH_SHORT).show();
                break;
            default:
                break;
        }
    }
}
```

### ImageView

> ImageView是用于在界面上展示图片的一个控件，它可以让我们的程序界面变的丰富多彩。

```xml
    <ImageView
        android:id="@+id/image_view"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/img_1"/>
```

- `android:src`: 可以通过该属性给ImageView指定一张图片。

我们还可以通过代码动态的更改ImageView中的图片，如下：

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private ImageView imageView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);

        // 获取imageView
        imageView = findViewById(R.id.image_view);
        // 使用实现接口的形式来注册监听器
        button1.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                imageView.setImageResource(R.drawable.img_2);
                break;
            default:
                break;
        }
    }
}
```

### ProgressBar

> ProgressBar用于在界面上显示一个进度条，表示我们的程序正在加载一些数据。

```xml
    <ProgressBar
        android:id="@+id/progress_bar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
```

- `android:visibility`: 所有的Android都具有这个属性，Android控件的可见属性。可选值有`visible`,`invisible`,`gone`:
  - `visible`: 表示控件可见;
  - `invisible`: 表示控件不可见，但是控件仍然占据原来的位置和大小;
  - `gone`：则表示控件不可见，且不再占用任何屏幕空间。

  我们还可以通过代码控制控件的可见性,使用的是`setVisibility()`方法，方法可传入`View.VISIBLE`, `VIEW.INVISIBLE`, `VIEW.GONE` .

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private ProgressBar progressBar;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);

        // 获取progressBar
        progressBar = findViewById(R.id.progress_bar);
        // 使用实现接口的形式来注册监听器
        button1.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                if (progressBar.getVisibility() == View.GONE) {
                    progressBar.setVisibility(View.VISIBLE);
                } else {
                    progressBar.setVisibility(View.GONE);
                }
                break;
            default:
                break;
        }
    }
}
```

我们还可以给ProgressBar设置不同的样式，可以将上面的圆形进度条，通过style属性将他指定为水平进度条。

还可以通过`andoird:max`属性给进度条设置一个最大值，然后在代码中动态的改变进度条的值。

```xml
    <ProgressBar
        android:id="@+id/progress_bar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        style="?android:attr/progressBarStyleHorizontal"
        android:max="100"/>
```

动态的改变水平进度条的值：

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private ProgressBar progressBar;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);


        // 获取progressBar
        progressBar = findViewById(R.id.progress_bar);

        // 使用实现接口的形式来注册监听器
        button1.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                // 设置水平进度条的值
                int progress = progressBar.getProgress();
                progress += 10;
                progressBar.setProgress(progress);
                break;
            default:
                break;
        }
    }
}
```

### AlertDialog

> AlertDialog可以在当前的界面弹出一个对话框，这个对话框置顶于所有的界面元素之上的，能够屏蔽掉其他控件的交互能力，因此AlertDialog一般用于提示一些非常重要的内容或者警告信息。

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);

        // 使用实现接口的形式来注册监听器
        button1.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                // 对话框
                AlertDialog.Builder dialog = new AlertDialog.Builder(MainActivity.this);
                // 设置对话框标题
                dialog.setTitle("This is Dialog");
                // 设置对话框内容
                dialog.setMessage("Something important.");
                // 可否用Back键关闭对话框
                dialog.setCancelable(false);
                // 设置确认按钮的点击事件
                dialog.setPositiveButton("OK", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                    }
                });
                // 设置取消按钮的点击事件
                dialog.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                    }
                });
                // 将对话框显示出来
                dialog.show();
                break;
            default:
                break;
        }
    }
}
```

### ProgressDialog

> ProgresDialog和AlertDialog有点类似，都可以在界面上弹出一个对话框，都能够屏蔽掉其他控件的交互能力。不同的是，ProgressDialog会在对话框中显示一个进度条，一般用于表示当前操作比较耗时，让用户耐心等待。

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button1 = findViewById(R.id.button1);


        // 使用实现接口的形式来注册监听器
        button1.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.button1:
                // ProgressDialog
                ProgressDialog progressDialog = new ProgressDialog(MainActivity.this);
                // 设置标题
                progressDialog.setTitle("This is ProgressDialog");
                // 设置内容
                progressDialog.setMessage("Loading...");
                // 设置是否可以通过Back键取消
                progressDialog.setCancelable(true);
                // 显示
                progressDialog.show();
                break;
            default:
                break;
        }
    }
}
```
