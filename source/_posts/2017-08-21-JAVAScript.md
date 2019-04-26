---
layout: post
title:  "JavaScript入门"
date:   2017-08-21 08:16:54
categories:
- JavaScript
tags: 
- JavaScript
---


JAVAScript一种直译式脚本语言,是一种动态类型.弱类型.基于原型的语言,内置支持类型.
它的解释器被称为JAVAScript引擎,为浏览器的一部分,广泛用于客户端的脚本语言,最早是在HTML网页上使用,用来给HTML网页增加动态功能.




### 组成部分

* ECMAScript,描述了该语言的基本语法.
* 文档对象模型(DOM),描述处理网页内容的方法和接口.
* 浏览器对象模型(BOM),描述与浏览器进行交互的方法和接口.

![](http://oujvmc3la.bkt.clouddn.com/js.jpg)

### 基本特点

JAVAScript是一种属于网络的脚本语言,已经被广泛用于Web应用开发,常用来为网页添加各种各样的动态效果,为用户提供更流畅美观的浏览效果.通常JAVAScript脚本是通过嵌入在HTML中来实现自身的功能的.

1. 是一种解释性脚本语言(代码不进行预编译).
2. 主要用来向HTML页面添加交互行为.
3. 可以直接嵌入HTML页面,但写成单独的js文件有利于结构和行为的分离.
4. 跨平台特性,在绝大多数浏览器的支持下,可以在多种平台下运行(如windows.Linux.Mac.Android.ios等)

### JAVAScript的作用

* 嵌入动态文本与HTMLyemain
* 对浏览器事件做出响应.
* 读写HTML元素.
* 在数据被提交到服务器之前验证数据.
* 检测访客的浏览器信息
* 控制cookies,包括创建和修改等.
* 基于Node.js技术进行服务器编程

### JAVAScript的编写方式

* 行内样式

    `<button onclick="alert();">点我</button>`

* 嵌套样式:这个样式是写在`<body></body>`之内的,而不是别的.

```
<!--嵌套javascript  -->
  <script>
    alert("我是提示框！！！！");
  </script>
```
* 外部的js脚本

  先要创建一个`.js`结尾的文件,在里面写你需要的js脚本.
  然后同样是在`<body></body>`标签中引入外部js脚本.
  `<script src="./text.js"></script>`

### javascript 基本概念

#### 严格区分大小写

第一个概念就是ECMAScript中的一切(变量,函数名和操作符)都区分大小写.
这也就意味着,变量名`test`和变量名`Test`分别代表着两个不同的变量.而函数名不能使用`typeof`,因为他是一个关键字,但`typeOf`则完全可以是一个有效的函数名.

#### 标识符

所谓标识符,就是指变量,函数,属性的名字,或者函数的参数.标识符可以使按照下列格式规则组合起来的一个或多个字符:

1. 第一个字符必须是一个字母,下划线(_)或一个美元符号($)其他字符可以是字母,下划线,美元符号或数字.

2. 标识符的字母也可以包含扩展的ASCII或Unicode字母字符,但我们不推荐这么做.

3. 按照惯例,ECMAScript 标识符采用驼峰大小写格式,也就是第一个字母小写,剩下的每个单词的首字母大写,例如:`firstChild`
4. 不要和关键字重名

#### 注释风格

1. 单行注释`//`,`//这里是单行注释!`

2. 多行注释`/* 这是注释内容! */`

```
/*
这里是注释!
*/
```

> 注意:不要和python,html中的注释混淆.

#### 语句

ECMAScript 中的语句以一个分号结尾;如果省略分号,则由解析器确定语句的结尾,比如:

```
var value1 = a - b  //不推荐
var value2 = a + b; //推荐
```

使用代码块,让代码看起来更加清晰,比如:

```
//不推荐
if (value)
  alert("value is ...")

//推荐
if (value){
  alert("value is ...")
}
```

#### 关键字

![](http://oujvmc3la.bkt.clouddn.com/keywords.png)

> 注意:定义标识符时不要使用关键字和保留字.

### JAVAScript 变量

ECMAScript 的变量是松散类型的,所谓松散类型就是可以用来保存任何类型的数据.换句话说,每个变量仅仅是一个用于保存值的占位符而已.定义变量时要使用`var`操作符.再次定义变量就不需要`var`操作符了.
比如:

```
//定义变量！！！！
    // var是个关键字，a是变量名(标识符)
    //注意标识符的命名规则
    var a = 10; //分号表示一条语句的结尾

    //用于控制台输出
    console.log(a);
```

### JAVAScript 数据类型

ECMAScript 中有 5 种简单数据类型(也称为基本数据类型): `Undefined`, `Null`, `Boolean`, `Number`和 `String` 。
对一个值使用 `typeof` 操作符可能返回下列某个字符串:

* “undefined” ——如果这个值未定义;
* “boolean” ——如果这个值是布尔值;
* “string” ——如果这个值是字符串;
* “number” ——如果这个值是数值;
* “object” ——如果这个值是对象或 null ;
* “function” ——如果这个值是函数。

例如:

```
var a = "string";
 alert(typeof a); //string

 var b = 123;
 alert(typeof b); //number

 var c = true;
 alert(typeof c);//boolean

 var d;
 alert(typeof d);//undefined

 var f = function(){alert("hello");};
 alert(typeof f);//function

 var e = null;
 alert(typeof e);//object    
```

#### 字符串类型

String 类型用于表示由零或多个 16 位 Unicode 字符组成的字符序列,即字符串。字符串可以由双引号(")或单引号(’)表示,因此下面两种字符串的写法都是有效的:

```
var a = "string1";
var b = 'string2';
```

> 注意:不要混合使用单引号或者双引号,要统一,否则会报错.

```
var str1 = "hello";
     var str2 = "world";
     var str3 = 'hehe';
     console.log(str1,str2,str3);
     console.log("10");

    //typeof 测试数据类型
    console.log(typeof str1);//string

    //再次使用变量时，不需要var
    str1 = "HELLO";
    console.log(str1);

    //字符串拼接 "+"
    var  newStr = str1 + str2;
    console.log(newStr);


    //其他类型如何转换为字符串, toString()!!!!
    a = 20;
    console.log(a.toString());//直接变为字符串
    console.log(a.toString(2));//将数字按照2进制转换为字符串， 10100

```

其他类型装换为字符串的方法`toString()`,比如:

```
var a = 10;
console.log(a.toString());
console.log(a.toString(2));//转换为２进制，“1010”
console.log(a.toString(8));//转化为8进制 “12”
console.log(a.toString(10));//转化为10进制 “10”
console.log(a.toString(16));//转化为16进制 “a”

var b = false;
console.log(b.toString());//false
```

在不知道要转换的值是不是`null`或`undefined`的情况下,还可以使用转型函数`String()`,这个函数能够将任何类型的值转换为字符串.

```
var d;
console.log(String(d));//undefined
var c = null;
console.log(String(c));//null
```

#### 布尔类型

boolean类型是`ECMAScript`中使用最多的一种类型,该类型只有两个字面值:`true`和`false`.
将一个值转换为其对应的`Boolean`值,可以调用转型函数`Boolean()`.

```
console.log(true);
console.log(false);
console.log(Boolean("hellowitwghjjhi"));//true
console.log(Boolean(""));//false
console.log(Boolean(10));//true
console.log(Boolean(-10));//true
console.log(Boolean(10.123));//true
console.log(Boolean(0));//false
```

![](http://oujvmc3la.bkt.clouddn.com/bool1.png)
![](http://oujvmc3la.bkt.clouddn.com/bool2.png)

#### 数值类型

最基本的数值格式是10进制整数,除了以10进制表示外,整数还可以通过8进制或16进制的字面值来表示.比如:

```
var g = 016;//8进制使用'0'开头
console.log(g);//14   控制台是以10进制输出的
var k = 0x16;//16进制,使用'0x'开头
console.log(k);//22
```

##### 浮点数

所谓浮点数值,就是该数值中必须包含一个小数点,并且小数点后面必须至少有一位数字.

```
var a = 0.123;
var b = .123; //不推荐
```

对于那些极大或极小的数值,可以用`e`表示法(即科学计数法)表示的浮点数值表示,比如:

```
var o = 12e2;
console.log(o);//1200
var p = 12e-2;
console.log(p);//0.12
```

浮点数的最高精度是17位小数,但是在进行算数计算时其精度远远不如整数.例如:
`0.1`+`0.2`的结果不是`0.3`,而是`0.30000000000000004`.这个小小的舍入误差会导致无法测量特定的浮点数值.

##### NaN

NaN,即非数值(Not a Number)是一个特殊的数值,这个数值用于表示一个本来要返回数值的操作数,未返回数值的情况(这样就不会抛出错误了).
ECMAScript定义了`isNaN()`函数.这个函数接受一个参数,该参数可以是任何类型,而函数会帮我们确定这个参数是否`不是数值`.

```
alert(isNaN(NaN));//true  arert这个函数会让浏览器弹出一个提示窗口
alert(isNaN(10));//false
alert(isNaN("blue"));//true
alert(isNaN("10"));//false
alert(isNaN(true));//false
```

> 注意:可以看出当`isNaN()`不是数值才会是true.因为有隐式转换的存在才会把`"10"`或者`""`看成数字.

#### 未定义类型(undefined)

Undefined 类型只有一个值,即`undefined`.在使用var声明变量但并未对其加以初始化时,这个变量就是`undefined`,或者显式的进行`undefined`赋值操作,比如:

```
var t;
console.log(typeof t);//undefined
var u = undefined;
console.log(typeof u);//underfined
```

对于尚未声明过得变量,只能执行一项操作,即使用`typeof`操作符检测其数据类型.

```
console.log(c);//出现错误，因为没有定义变量c
console.log(typeof c); //undefined
```

#### Null类型

Null类型是第二个只有一个值的数据类型,这个特殊的值是`null`.从逻辑角度来看.`null`值表示一个空对对象指针,而这也是使用`typeof`操作符检测`null`值时会返回`object`的原因,比如:

```
var a = null;
console.log(typeof  a);//object
```

#### JAVAScript 函数

函数对于任何语言来说都是一个核心的概念.通过函数可以封装任意多条语句,而且可以在任何地方.任何时候调用执行.

```
function kai(arg1,arg2){
  return arg1 + arg2;
}

k = kai(2,3)
console.log(k);//5
```

js中的函数有个特点:声明提升.
函数可以定义在可调用的任何位置,在javascript引擎中解释运行该代码时,会自动将函数提升到该作用域的顶部.例如:

```
fun();

function fun(){
  console.log("fun......");
}
```

#### 变量作用域

按作用域分有两种变量:全局变量,局部变量.在JAVAScript中只有函数是用于区分作用域的.

```
var a = 10; //全局变量

function fun() {
  var b = 20;
  //局部变量
  //局部变量只能在指定的作用内访问
  console.log("b = ", b);
}

//全局变量在任意位置都可以访问
console.log("a = ", a);

fun();
```
