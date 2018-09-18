---
title: interface
date: 2018-09-18 20:37:57
tags:
  - interface
categories:
  - JAVA
---

在java中，接口表示一种规范/约束/要求实现者必须遵循该规范：用来约束使用者应该怎么做。

接口只定义了类应当遵循的规范，却不关心这些类的内部数据和其功能的实现细节。站在程序的角度上说接口只规定了类里必须提供的方法，从而分离了规范和实现，增强了系统的可拓展性和可维护性。

# 接口的定义

```java
interface Usbable{
    void transData();
}
```

> 接口其实是一个特殊的抽象类,内部的`abstract`可以不写

> 如果没有在方法前面加上`abstract`,会在编译时自动添加上`public abstract void trabsData()`

# 接口中的注意点

>> 1.接口中是没有构造器的,接口不能创建对象;
>
>> 2.接口中定义常量,都是全局的静态常量;
>>> `String name = "world";` >>
>>> `public static final String name = "world";`
>
>> 3.接口中定义的方法都是公共的抽象的方法;
>>> `void transData();` >>
>>> `public abstract void transData();`
>
>> 4.接口中可以定义内部类,内部类也是公共的抽象的;

# 接口的继承

```java
package 接口;

//爬行动物
interface Iwalkable{
	void walk();
}
//水生动物
interface Iswimable{
	void swim();
}
//两栖动物
interface Iamphibiousablbe extends Iswimable,Iwalkable{
	
}
```

> 接口是可以继承的,它是可多继承的.

# 接口的实现

```java
package 接口;

//爬行动物
interface Iwalkable{
	void walk(); //public abstract void walk();
}
//水生动物
interface Iswimable{
	void swim();
}
//两栖动物
interface Iamphibiousablbe extends Iswimable,Iwalkable{
	
}

class Aniaml{
	String name;
	String color;
}


//Cat实现了Iwalkable接口
// 实现关系有时候也称它为继承关系
class Cat implements Iwalkable{
	//覆盖的时候，子类的方法权限要比父类大，或者一样大
	public void walk() {
		System.out.println("走猫步");
	}
}

public class _01Test {

	public static void main(String[] args) {
		// (Iwalkable接口也可以称为它的父类)
		//多态写法，把子类对象赋值给父类类型
		Iwalkable cat = new Cat(); //面向接口编程
		cat.walk();//多态运行时表现出来的还是子类的特征。（编译时看左边，运行时看右边）

	}

}

```