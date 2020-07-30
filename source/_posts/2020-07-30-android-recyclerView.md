---
title: 更强大的滚动组件 RecyclerView
date: 2020-07-30 10:37:22
tags:
  Android Java
category:
  Android
---

> `ListView`并不是完全没有缺点，我们不使用一些技巧来提升它的运行效率，那么`ListView`的效率会非常差，还有`ListView`的拓展性也不够好，它只能实现数据的纵向滚动效果，如果我们要实现横向滚动的话，`ListView`是做不到的。

> 为此，Android提供了一个更强大的滚动控件--`RecyclerView`,优化了`ListView`的各种不足之处。

### RecyclerView的基本用法

> 想要使用`RecyclerView`需要在项目的`build.gradle`中添加相应的依赖库才可以。

```
implementation 'com.android.support:recyclerview-v7:28.0.0'
```

> 在布局中加入`RecyclerView`,由于不是系统内置组件，需要写出完整的包路径。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
     android:layout_width="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</LinearLayout>
```

> 新建适配器

```java
public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
    private  List<Fruit> mFruitList;

    static class ViewHolder extends RecyclerView.ViewHolder {
        ImageView fruitImage;
        TextView fruitName;
        public ViewHolder(View v) {
            super(v);
            fruitImage = (ImageView) v.findViewById(R.id.fruit_image);
            fruitName = (TextView) v.findViewById(R.id.fruit_name);
        }
    }

    public FruitAdapter(List<Fruit> fruitList) {
        this.mFruitList = fruitList;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.fruit_item, parent, false);
        ViewHolder holder = new ViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        Fruit fruit = mFruitList.get(position);
        holder.fruitImage.setImageResource(fruit.getImageId());
        holder.fruitName.setText(fruit.getName());
    }

    @Override
    public int getItemCount() {
        return mFruitList.size();
    }
}
```

> `ViewHolder`是我们定义的一个内部类，在构造函数传入一个参数，这个参数通常是`RecyclerView`子项的最外层布局；

> 我们新建的`FruitAdapter`的构造函数用于把要展示的数据传进来。
> 由于`FruitAdapter`继承自`RecyclerView.Adapter`，那么就必须重写`onCreateViewHolder()`,`onBindViewHolder()`,`getItemCount()`：
> - `onCreateViewHolder()`: 用于创建ViewHolder实例的，这个函数把布局加载进来，然后创建一个`ViewHolder`实例。
> - `onBindViewHolder()`: 用于对`RecyclerView`的子项进行赋值，会在每个子项被滚动到屏幕内的时候执行。
> - `getItemCount()`: 用于告诉`RecyclerView`有多少个子项。


```java
public class RecyclerViewActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recycler_view);
        this.initFruits();
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        // 用于指定RecyclerView的布局方式，LinearLayoutManager是线性布局
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        recyclerView.setLayoutManager(layoutManager);
        // 创建适配器
        FruitAdapter fruitAdapter = new FruitAdapter(fruitList);
        // 完成适配器设置
        recyclerView.setAdapter(fruitAdapter);
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

之后会实现和`ListView`类似的效果：

![](http://imgs.qipo.net/android/RecyclerView.jpeg)



### 横向滚动和瀑布流布局



首先要对布局进行修改,因为目前这个布局中的元素都是水平排列的,适用于纵向滚动的场景，而我们要实现横向滚动的话，应该将布局中的元素改为垂直排列比较合理。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="100dp"
    android:layout_height="wrap_content">


    <ImageView
        android:id="@+id/fruit_image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        />

    <TextView
        android:id="@+id/fruit_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="10dp"
        />
</LinearLayout>
```

接下来修改Activity

```java
public class RecyclerViewActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recycler_view);
        this.initFruits();
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        LinearLayoutManager layoutManager = new LinearLayoutManager(this);
        // 设置为水平布局
        layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
        recyclerView.setLayoutManager(layoutManager);
        FruitAdapter fruitAdapter = new FruitAdapter(fruitList);
        recyclerView.setAdapter(fruitAdapter);
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

> 只加入了一行代码

```java
layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);
```

> 调用`LinearLayoutManger`的`setOrientation()`方法来设置让布局横向排列，默认为纵向排列。

实现效果如下：

![](http://imgs.qipo.net/android/RecyclerViewHorzontal.jpg)

实现瀑布流布局：

首先修改布局文件:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="5dp"
    >


    <ImageView
        android:id="@+id/fruit_image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        />

    <TextView
        android:id="@+id/fruit_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="left"
        android:layout_marginTop="10dp"
        />
</LinearLayout>
```

>  修改宽度为`match_parent`,宽度应该是根据布局的列数来自动适配的，而不是一个固定值。
>
> `android:layout_margin="5dp"`应该让子项之间保持一定的间距，这样不至于所有子项都紧贴在一起。

然后修改Activity中的代码:

```java
public class RecyclerViewActivity extends AppCompatActivity {

    private List<Fruit> fruitList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_recycler_view);
        this.initFruits();
        RecyclerView recyclerView = findViewById(R.id.recycler_view);
        StaggeredGridLayoutManager staggeredGridLayoutManager = new StaggeredGridLayoutManager(3, StaggeredGridLayoutManager.VERTICAL);
        recyclerView.setLayoutManager(staggeredGridLayoutManager);
        FruitAdapter fruitAdapter = new FruitAdapter(fruitList);
        recyclerView.setAdapter(fruitAdapter);
    }

    private void initFruits() {
        for (int i = 0; i < 5; i ++) {
            Fruit apple = new Fruit(getRandomLengthName("apple"),R.drawable.apple);
            fruitList.add(apple);
            Fruit pear = new Fruit(getRandomLengthName("pear"), R.drawable.pear);
            fruitList.add(pear);
            Fruit banana = new Fruit(getRandomLengthName("banana"), R.drawable.banana);
            fruitList.add(banana);
        }
    }

    private String getRandomLengthName(String name) {
        Random random = new Random();
        int length = random.nextInt(20) + 1;
        StringBuilder builder = new StringBuilder();
        for (int i = 0; i < length; i ++) {
            builder.append(name);
        }
        return builder.toString();
    }
}
```

> 更换为新的布局排列方式，RecyclerView一共有提供三种内置的布局排列方式: `StaggeredGridLayoutManager`,`GridLayoutManager`,`LinearLayoutManger`

```java
 StaggeredGridLayoutManager staggeredGridLayoutManager = new StaggeredGridLayoutManager(3, StaggeredGridLayoutManager.VERTICAL);
      recyclerView.setLayoutManager(staggeredGridLayoutManager);
```

> `StaggeredGridLayoutManager`
> 第一个参数用于指定布局的列数，传入3表示会把布局分为3列；
>
> 第二个参数用于指定布局的排列方向，传入`StaggeredGridLayoutManager.VERTICAL`表示会让布局纵向排列；

实现效果如下：

![](http://imgs.qipo.net/android/RecyclerView1.jpg)

### RecyclerView的点击事件

>  修改Adapter中的代码

```java
public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
    private  List<Fruit> mFruitList;

    static class ViewHolder extends RecyclerView.ViewHolder {
        View fruitView;
        ImageView fruitImage;
        TextView fruitName;
        public ViewHolder(View v) {
            super(v);
            fruitView = v;
            fruitImage = (ImageView) v.findViewById(R.id.fruit_image);
            fruitName = (TextView) v.findViewById(R.id.fruit_name);
        }
    }

    public FruitAdapter(List<Fruit> fruitList) {
        this.mFruitList = fruitList;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.fruit_item, parent, false);
//        ViewHolder holder = new ViewHolder(view);
        final ViewHolder holder = new ViewHolder(view);
        holder.fruitView.setOnClickListener((v) -> {
            int position = holder.getAdapterPosition();
            Fruit fruit = mFruitList.get(position);
            Toast.makeText(v.getContext(), "you clicked view" + fruit.getName(), Toast.LENGTH_SHORT).show();
        });
        holder.fruitImage.setOnClickListener((v) -> {
            int position = holder.getAdapterPosition();
            Fruit fruit = mFruitList.get(position);
            Toast.makeText(v.getContext(), "you click image" + fruit.getName(), Toast.LENGTH_SHORT).show();
        });

        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        Fruit fruit = mFruitList.get(position);
        holder.fruitImage.setImageResource(fruit.getImageId());
        holder.fruitName.setText(fruit.getName());
    }

    @Override
    public int getItemCount() {
        return mFruitList.size();
    }
}
```

> 在`ViewHolder`中添加`furitView`来保存子项最外层布局的实例，然后在`onCreateViewHolder()`方法中注册点击事件就可以了。在点击事件中我们先拿到用户点击的`position`，然后通过`position`拿到对应的`Fruit`实例，再使用`Toast`分别弹出两种不同的内容以示区分。

![](http://imgs.qipo.net/android/RecyclerViewClick.jpg)

