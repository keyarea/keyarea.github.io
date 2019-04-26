---
title: 接口
date: 2018-09-18 20:37:57
tags:
  - Java
categories:
  - Java
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

# 接口与抽象类的区别

## 相同点

1. 都是被其它类实现或者被继承的；
2. 都不能实例化；
3. 都可以定义抽象方法，定义的抽象方法子类必须得要覆盖；

## 不同点

1. 接口是没有构造器的，抽象类中有构造器；
2. 抽象类中可以包含抽象方法和普通方法，接口当中只能有抽象方法,不能有普通方法（带有方法体）；（jdk8以后可以有普通方法）；
3. 接口当中成员变量为`public static final 变量;`->静态常量;抽象类中默认的权限(包访问权限);
4. 方法 接口中默认方法为`public abstract 方法名;`;抽象类中默认的权限(包访问权限);


# 面向接口编程

> 面向接口编程:把实现类对象赋值给接口类型的变量;

> 多态的好处：屏蔽了不同类之间的实现差异，从而达到通用编程；

```java
package 接口;

interface IUSB {
	void swapData();
}

class Mouse implements IUSB {

	@Override
	public void swapData() {
		System.out.println("鼠标移动");

	}

}

class Keyboard implements IUSB {

	@Override
	public void swapData() {
		System.out.println("输出字符");

	}

}

class MotherBoard {
	public void pluginIn(IUSB USB) {
		USB.swapData();
	}
}

public class Test {
	public static void main(String[] args) {
		IUSB m = new Mouse();
		IUSB k = new Keyboard();

		MotherBoard mo = new MotherBoard();
		mo.pluginIn(k);
		mo.pluginIn(m);

	}
}
```
