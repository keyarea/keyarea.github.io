---
title: Android的四种基本布局
date: 2020-07-30 08:27:24
tags:
  Android Java
category:
  Android
---

## 四种基本布局

> 布局是一种可用于放置很多控件的容器，它可以按照一定的规律调整内部控件的位置，从而编写处精美的页面。当然，除了放置控件之外，也可以放置布局，通过多层布局的嵌套，我们就能够完成一些比较复杂的界面实现。

### 线性布局

> `LinearLayout`又被称之为线性布局，是一种非常常用的布局。正如他名字所描述的那样，这个布局会将它所包含的控件在线性方向上依次排列。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 3"/>

</LinearLayout>
```

> `android:orientation`该属性指定排列的方向，`veritical`: 指定了垂直方向上排列，`horizontal`: 指定了水平方向上排列。默认的排列方向则是`horizontal`水平方向排列。

布局中的对齐方式怎么设置呢？如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:orientation="vertical">

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1"
        android:layout_gravity="left"
        />

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="Button 2"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:text="Button 3"/>

</LinearLayout>
```

> `android:layout_gravity`和之前的`android:gravity`属性有些类似，但`android:gravity`用于指定文字在控件中的对齐方式，而`andoird:layout_gravity`用于指定控件在布局中的对齐方式。属性值都差不多；

> 但是需要注意的是，当布局的排列方向为`vertical`，只有水平方向的对齐方式是有效的，同理，当布局的排列方向为`horizontal`，只有垂直方向的对齐方式是有效的。

`android:layout_weight`，这个属性允许我们使用比例的方式来指定控件的大小

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:orientation="horizontal">


    <EditText
        android:id="@+id/input_message"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:hint="Type somethine"/>

    <Button
        android:id="@+id/send"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="send"/>

</LinearLayout>
```

> 这里我们将两个控件的宽度均设置为`0dp`,由于我们使用了`andoird:layout_weight`属性，此时的控件宽度不再由`android:layout_width`来决定。

> 在`EditText`和`Button`里都将`android:layout_weight`设置为1，这表示`EditText`和`Button`将在水平方向上评分宽度。

> 原理： 系统会把`LinearLayout`下所有控件指定的`layout_weight`值相加，得到一个总值，然后每个控件所占的大小的比例就是用该控件的`layout_weight`值除以刚才算出的总值。

### 相对布局

> `RelativeLayout`又被称作相对布局，也是一种非常常用的布局。它可以通过相对定位的方式让控件出现在布局的任何位置。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignParentLeft="true"
        android:text="Button 1"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:text="Button 2"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Button 3"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentLeft="true"
        android:text="Button 4"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:text="Button 5"/>

</RelativeLayout>
```

> 以上`android:layout_alignParentRight`...都是相对于父布局进行定位的。那控件可不可以相对于控件进行定位呢？

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Button 1"/>

    <Button
        android:id="@+id/button5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/button4"
        android:layout_toLeftOf="@id/button4"
        android:text="Button 2"/>

    <Button
        android:id="@+id/button6"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/button4"
        android:layout_toRightOf="@id/button4"
        android:text="Button 3"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/button4"
        android:layout_toLeftOf="@id/button4"
        android:text="Button 4"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/button4"
        android:layout_toRightOf="@id/button4"
        android:text="Button 5"/>

</RelativeLayout>
```

>-  `android:layout_above`: 该控件在目标控件的上方；
>- `android:layout_below`: 该控件在目标控件的下方；
>- `android:layout_toRightOf`: 该控件在目标控件的右边；
>- `android:layout_toLeftOf`: 该控件在目标控件的左边；

> 注意：当一个控件去引用另一个控件的id时，该控件一定要定义在引用控件的后面，不然会出现找不到id的情况。

还有另一组相对于控件进行定位的属性：

- `android:layout_alignLeft`: 表示让一个控件的左边缘和另一个控件的左边缘对齐；
- `android:layout_alignRight`: 表示让一个控件的右边缘和另一个控件的右边缘对齐；
- `android:layout_alignTop`: 表示让一个控件的头部和另一个控件对齐；
- `android:layout_alignBottom`: 同理。

### 帧布局 

> `FrameLayout`又被称为帧布局，它相对于前面两种布局就简单太多了。这种布局没有方便的定位方式，所有的控件都会默认摆放在布局的左上角。

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent">
    <TextView
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:text="This is TextView"
        />

    <ImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:src="@mipmap/ic_launcher"/>
</FrameLayout>
```

> 默认摆放在左上角，我们可以通过`androiod:layout_gravity`调节控件的对齐方式，将其放置在右上角。由于定位方式的欠缺，应用场景比较少。

### 百分比布局

> 在这种布局中，不再使用`wrap_content`,`match_parent`等方式来指定空间的大小，而是允许指定控件在布局中所占的百分比，这样的话可以轻松的实现平分布局甚至是任意比例分割布局的效果了。

> 由于`LinearLayout`本身 已经支持按比例指定控件的大小了，因此百分比布局只为`FrameLayout`和`RelativeLayout`进行了功能扩展，提供了`PercentFrameLayout`和`PercentRelativeLayout`这两个全新布局。

> 不同于以上三种布局，百分比布局属于新增布局。Android团队将百分比布局定义在了support库中，我们只需要在项目的`build.gradle`中添加百分比布局的依赖，就能保证百分比布局在Android所有系统版本上的兼容性了。

```groovy
dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation 'androidx.appcompat:appcompat:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    implementation 'com.android.support:percent:27.1.1'
    testImplementation 'junit:junit:4.13'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
}
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.percentlayout.widget.PercentFrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    >
    <Button
        android:text="Button1"
        android:layout_gravity="left|top"
        app:layout_widthPercent="50%"
        app:layout_heightPercent="50%"
        />

    <Button
        android:text="Button2"
        android:layout_gravity="right|top"
        app:layout_widthPercent="50%"
        app:layout_heightPercent="50%"
        />

    <Button
        android:text="Button3"
        android:layout_gravity="left|bottom"
        app:layout_widthPercent="50%"
        app:layout_heightPercent="50%"
        />

    <Button
        android:text="Button4"
        android:layout_gravity="right|bottom"
        app:layout_widthPercent="50%"
        app:layout_heightPercent="50%"
        />

</androidx.percentlayout.widget.PercentFrameLayout>
```

> 最外层我们使用了`PercentFrameLayout`，由于百分比布局并不是内置在系统SDK当中的，所以需要把完整的包名路径写出来。然后还必须定义一个app的命名空间，这样才能 使用百分比布局的自定义属性。

- `app:layout_widthPercent`: 该属性将各个按钮的宽度设置为布局的50%;
- `app:layout_heightPercent`:该属性将各个按钮的高度设置为布局的50%。