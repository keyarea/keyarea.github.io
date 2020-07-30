---
title: Android自定义控件
date: 2020-07-30 10:25:37
tags:
  Android Java
category:
  Andorid
---


> 我们所用的所有控件都是直接或者间接继承自View的，所用的所有布局都是直接或者间接继承自ViewGroup的。
>
> View是Android中最基本的一种UI组件，它可以在屏幕上绘制一块矩形区域，并能响应这块区域的各种事件，因此我们使用的各种控件其实就是在View的基础之上添加了各自特有的功能。
>
> ViewGroup则是一种特殊的View，它可以包含很多子View和子ViewGroup，是一个用于放置控件或者布局的容器。

### 引入布局

在`layout`目录下新建一个标题栏的布局文件`title.xml`,如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@drawable/ic_launcher_background">

    <Button
        android:id="@+id/title_back"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:text="Back"
        android:textColor="#fff"
        />

    <TextView
        android:id="@+id/title_text"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_weight="1"
        android:gravity="center"
        android:text="Title Text"
        android:textColor="#fff"
        android:textSize="24dp"
        />

    <Button
        android:id="@+id/title_edit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:text="Edit"
        android:textColor="#fff"
        />
</LinearLayout>
```

然后我们就可以在任何的布局文件中引用这个布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ViewActivity">

    <include layout="@layout/title"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

使用这种方式我们就不需要重复的去写布局文件，只需要一行include语句就可以了。

我们还需要把系统自带的标题栏隐藏掉:

```java
public class ViewActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_view);
        ActionBar actionBar = getSupportActionBar();
        if (actionBar != null) {
            actionBar.hide();
        }
    }
}
```

### 创建自定义控件

> 引入布局的技巧确实解决了重复编写布局代码的问题，但是如果布局中有一些控件要求能够响应事件，我们还需要在每个活动中为这些控件单独编写一次事件注册的代码。这个我们可以使用自定义控件的方式来解决：

```java
public class TitleLayout extends LinearLayout {
    public TitleLayout(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        // 对标题栏布局进行动态加载
        LayoutInflater.from(context).inflate(R.layout.title, this);
        Button titleBack = findViewById(R.id.title_back);
        Button titleEdit = findViewById(R.id.title_edit);
        titleBack.setOnClickListener((v)-> {
            ((Activity) getContext()).finish();
        });
        titleEdit.setOnClickListener((v)-> {
            Toast.makeText(context, "You click edit", Toast.LENGTH_SHORT).show();
        });
    }
}
```

如何在布局文件中添加这个自定义控件？

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ViewActivity">

    <net.qipo.viewstudy.TitleLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

> 这样返回按钮和编辑按钮的点击事件就已经自动实现好了，省去了很多编写重复代码的工作。
