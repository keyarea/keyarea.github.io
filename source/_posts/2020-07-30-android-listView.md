---
title: Android ListView控件
date: 2020-07-30 10:28:32
tags:
  Android Java
category:
  Android
---


> ListView绝对是Android上最常用的控件之一，几乎所有的应用程序都会用到它。ListView允许用户通过手指上下滑动的方式将屏幕外的数据滚动到屏幕内，同时屏幕上原有的数据则会滚动出屏幕。

### ListView的简单用法

> 加入ListView控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ListViewActivity">

    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

> 既然ListView是用于展示大量数据的，那我们应该把数据提供好。不过，数组中的数据时无法直接传递给ListView的，我们需要借助适配器来完成。Android中提供了很多适配器的实现类，其中我们使用的是ArrayAdapter，他可以通过泛型来指定要适配的数据类型，然后在构造函数中把要适配的数据传入。

```java

public class ListViewActivity extends AppCompatActivity {

    private String[] data = { "apple", "pear", "banana", "orange", "watermelon", "grape",
            "pineapple" , "strawberry", "cherry", "mango", "apple", "pear", "banana", "orange", "watermelon", "grape",
            "pineapple" , "strawberry", "cherry", "mango" };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_view);
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(
                ListViewActivity.this, android.R.layout.simple_list_item_1, data
        );
        ListView listView = findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }
}
```

### 定制ListView的界面

> 如何如何对ListView界面进行定制呢？

首先定义实体类，作为ListView适配器的适配类型。

```java
public class Fruit {
    private  String name;
    private int imageId;

    public Fruit(String name, int imageId) {
        this.name = name;
        this.imageId = imageId;
    }

    public String getName() {
        return name;
    }

    public int getImageId() {
        return imageId;
    }
}

```

然后为 ListView的子项指定一个我们自定义的布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">


    <ImageView
        android:id="@+id/fruit_image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/fruit_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:layout_marginStart="10dp"
        />
</LinearLayout>
```

接下来创建一个自定义的适配器，这个适配器继承自ArrayAdapter，并将泛型指定为Fruit类


```java
public class FruitAdapter extends ArrayAdapter {
    private int resourceId;
    public FruitAdapter(@NonNull Context context, int resource, @NonNull List<Fruit> objects) {
        super(context, resource, objects);
        resourceId = resource;
    }

    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        Fruit fruit = (Fruit) getItem(position); // 获取当前项的Fruit实例
        // false 表示只让我们在父布局中声明的layout属性生效，但不会为这个View添加父布局，因为一旦View有了父布局之后，它就不能再添加到ListView中了
        View view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        TextView fruitName = view.findViewById(R.id.fruit_name);
        ImageView fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
        fruitImage.setImageResource(fruit.getImageId());
        fruitName.setText(fruit.getName());
        return view;
    }
}
```

> 重写了父类的构造函数，用于将上下文，ListView子项布局的id和数据都传递进来。
>
> 重写了`getView()`方法。这个方法在每个子项被滚动到屏幕内的时候被调用。在该方法中，首先通过`getIteam()`方法得到当前项的Fruit实例，然后通过LayoutInflater来为这个子项加载我们传入的布局。

```java
public class ListViewActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_view);
        // 初始化水果数据
        initFruits();
        FruitAdapter adapter = new FruitAdapter(ListViewActivity.this, R.layout.fruit_item, fruitList);
        ListView listView  = findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }

    private void initFruits() {
        for (int i = 0; i < 2; i ++) {
            Fruit apple = new Fruit("Apple",R.drawable.apple);
            fruitList.add(apple);
            Fruit pear = new Fruit("Pear", R.drawable.pear);
            fruitList.add(pear);
            Fruit banana = new Fruit("Banana", R.drawable.banana);
            fruitList.add(banana);
        }
    }
}
```

运行程序，效果如下图所示。

![](http://imgs.qipo.net/android/listView.jpg)



### 提升ListView的运行效率

> 目前`ListView`运行效率是很低的，因为在`FruitAdapter`的`getView()`方法中，每次都将布局重新加载了一遍。
>
> `getView()`方法中还有一个`convertView`参数，这个参数用于将之前加载好的布局进行缓存，以便之后可以进行重用。

```java
public class FruitAdapter extends ArrayAdapter {
    private int resourceId;
    public FruitAdapter(@NonNull Context context, int resource, @NonNull List<Fruit> objects) {
        super(context, resource, objects);
        resourceId = resource;
    }

    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        Fruit fruit = (Fruit) getItem(position); // 获取当前项的Fruit实例
        View view;
        if (convertView == null) {
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
        } else {
            view = convertView;
        }
        TextView fruitName = view.findViewById(R.id.fruit_name);
        ImageView fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
        fruitImage.setImageResource(fruit.getImageId());
        fruitName.setText(fruit.getName());
        return view;
    }
}
```

> 我们现在在`getView()`方法中进行判断，如果`convertView`为`null`，则使用`ViewInflater`去加载布局，如果不为`null`则直接对`convertView`进行重用。

但是每次`getView()`的时候还是会调用View的`findViewById()`方法来获取一次控件的实例。可以借助ViewHolder来对这部分进行性能优化。

```java
public class FruitAdapter extends ArrayAdapter<Fruit> {
    private int resourceId;
    public FruitAdapter(@NonNull Context context, int resource, @NonNull List<Fruit> objects) {
        super(context, resource, objects);
        resourceId = resource;
    }

    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        Fruit fruit = getItem(position); // 获取当前项的Fruit实例
        View view;
        ViewHolder viewHolder;
        if (convertView == null) {
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
            viewHolder = new ViewHolder();
            viewHolder.fruitImage = view.findViewById(R.id.fruit_image);
            viewHolder.fruitName = view.findViewById(R.id.fruit_name);
            view.setTag(viewHolder); // 将ViewHolder存储在view中
        } else {
            view = convertView;
            viewHolder = (ViewHolder) view.getTag(); // 重新获取ViewHolder
        }
        viewHolder.fruitName.setText(fruit.getName());
        viewHolder.fruitImage.setImageResource(fruit.getImageId());
        return view;
    }
}

class ViewHolder {
    ImageView fruitImage;
    TextView fruitName;
}
```

> 这样所有控件的实例都缓存到了ViewHolder中，就没有必要每次都通过`findViewById()`方法来获取控件实例了。

### ListView的点击事件

> ListView如何才能响应用户点击

```java
public class ListViewActivity extends AppCompatActivity {

//    private String[] data = { "apple", "pear", "banana", "orange", "watermelon", "grape",
//            "pineapple" , "strawberry", "cherry", "mango", "apple", "pear", "banana", "orange", "watermelon", "grape",
//            "pineapple" , "strawberry", "cherry", "mango" };

    private List<Fruit> fruitList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_list_view);
        // 初始化水果数据
        initFruits();
        FruitAdapter adapter = new FruitAdapter(ListViewActivity.this, R.layout.fruit_item, fruitList);
        ListView listView  = findViewById(R.id.list_view);
        listView.setAdapter(adapter);
        // 点击任何一个子项，都会回调该方法
        // 通过positon参数可以判断出用户点击的是哪个子项
        listView.setOnItemClickListener((parent, view, position, id) -> {
            Fruit fruit = fruitList.get(position);
            Toast.makeText(ListViewActivity.this, fruit.getName(), Toast.LENGTH_SHORT).show();
        });
    }

    private void initFruits() {
        for (int i = 0; i < 5; i ++) {
            Fruit apple = new Fruit("Apple",R.drawable.apple);
            fruitList.add(apple);
            Fruit pear = new Fruit("Pear", R.drawable.pear);
            fruitList.add(pear);
            Fruit banana = new Fruit("Banana", R.drawable.banana);
            fruitList.add(banana);
        }
    }
}
```
