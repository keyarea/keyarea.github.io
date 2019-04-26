---
title: 包装类
date: 2018-09-14 21:21:39
tags:
  - Java
categories:
  - Java
---

# 什么是包装类

包装类（Wrapper Class）： Java是一个面向对象的编程语言，但是Java中的八种基本数据类型却是不面向对象的，为了使用方便和解决这个不足，在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八种基本数据类型对应的类统称为包装类(Wrapper Class)，包装类均位于java.lang包。

# 包装类的用途

1. 为了使用方便Java中将8中基本数据类型进行了封装：除了Integer和Character类以后，其它六个类的类名和基本数据类型一直，只是类名的第一个字母大写即可。

```
    boolean —>  Boolean
    char    —>  Character
    byte    —>  Byte
    short   —>  Short
    long    —>  Long
    int     —>  Integer
    float   —>  Float
    double  —>  Double
```

2. 对于包装类说，用途主要包含两种：

   > a、作为 和基本数据类型对应的类 类型存在，方便涉及到对象的操作。

   > b、包含每种基本数据类型的相关属性如最大值、最小值等，以及相关的操作方法。


# 包装类的实际使用

> 装箱：将 基本数据类型 封装为对象类型，来符合java的面向对象的思想。

> 拆箱：将对象重新转化为基本数据类型。

## 装箱操作

```java
public class Test {

	public static void main(String[] args) {
		int a = 10;
		//把基本数据类型变成包装类  >> 装箱操作
		//之所以输出10，是因为Integer内部覆写了toString
		Integer num = new Integer(a);
		Integer num1 = Integer.valueOf(20);

		System.out.println(num); //10
		System.out.println(num1); //20
		//int类型能够表示的最大值
		System.out.println(num.MAX_VALUE); //2147483647
		//int类型能够表示的最小值
		System.out.println(num.MIN_VALUE); //-2147483648
		//表示基本类型int的class实例
		System.out.println(num.TYPE); //int
	}

}
```

## 拆箱操作

```java
public class Test {

	public static void main(String[] args) {
		int a = 10;
		//把基本数据类型变成包装类  >> 装箱操作
		Integer num = Integer.valueOf(a);

		//拆箱操作：把包装类的对象，转换为对应的基本数据类型的变量
		int num1 = num.intValue();

		System.out.println(num); //10  对象
		System.out.println(num1); //10 基本数据类型		
	}
}
```

## 自动拆装箱

JDK自从1.5版本以后，就引入了自动拆装箱的语法，也就是在进行基本数据类型和对应的包装类转换时，系统将自动进行，这将大大方便程序员的代码书写。

```java
public class Test {

	public static void main(String[] args) {
		//5是基本数据类型，通过自动装箱变为对象类型
		//编译器执行了Integer num = Integer.valueOf(5)
		Integer num = 5;

		//自动拆箱，实际上执行了 int num1 = num.intValue()
		int num1 = num;
		System.out.println(num1); //5

	}

}
```

> valueOf（）源码分析：

```java
public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

> 说明：Integer num = 5;相当于编译器执行了Integer num = Integer.valueOf(5)操作。这个方法就是返回一个 Integer 对象，只是在返回之前，看作了一个判断，判断当前 i 的值是否在 [-128,127] 区别，且 IntegerCache 中是否存在此对象，如果存在，则直接返回引用，否则，创建一个新的对象。

# 包装类的缓存值

> 各个包装类缓存值范围 ：

```
boolean：true和false
byte：-128～127
char：0～127
short：-128～127
int：-128～127
long：-128～127
```

> 特别注意：对于float和double没有缓存。

>在对上述值进行装箱的时候（如：Character c1=127;or Integer i= Integer.valueOf(1); 这个不行哦：Integer i2=new Integer(1)；因为这是新建了一个对象出来。）,并不是创建一个新对象而是使用缓存中的对象，如果超出范围则需要新建立对象。

# Int与Integer的区别？

1. int 是基本类型，直接存数值；而integer引用数据类型。
2. Int的声明不需要实例化，且变量声明后的初始值为0；Integer的是一个类，初始值为null，需要进行实例化，才能对变量数据进行处理。
3. Integer类是int的包装类，实际开发中Integer被看成一个对象，可以进行数据转换等操作。

# 判断下面程序片段输出的结果：

```java
        Integer num1 = 200;   
        Integer num2 = 200;           
        System.out.println("num1==num2: "+(num1==num2));                    
        Integer num3 = 100;   
        Integer num4 = 100;   
        System.out.println("num3==num4: "+(num3==num4));
```

输出结果：num1==num2:false 和num3==num4:true

>说明：
>
>>首先要明确equal方法与==的区别：
equals() 比较的是两个对象的值（内容）是否相同。
“==” 比较的是两个对象的引用（内存地址）是否相同，也用来比较两个基本数据类型的变量值是否相等。
>
>>java定义：在自动装箱时对于值从–128到127之间的值，它们被装箱为Integer对象后，会存在内存中被重用，始终只存在一个对象而如果超过了从–128到127之间的值，被装箱后的Integer对象并不会被重用，即相当于每次装箱时都新建一个 Integer对象；
