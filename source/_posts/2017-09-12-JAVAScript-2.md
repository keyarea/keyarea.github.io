---
layout: post
title:  "JAVAScript基础进阶"
date:   2017-09-12 08:16:54
categories:
- JAVAScript
tags:
- JAVAScript
---



JavaScript声明提升
JavaScript数值转换
javascript操作符及控制语句
javascript函数传参
javascript函数操作符
javascript数组的应用
javascript原生DOM操作
javascript原生事件操作







## 声明提升

### 变量声明提升

变量声明提升:变量的定义在执行时会被提升到当前作用域的顶部，但是赋值的操作位置不变.

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>javascript</title>
  </head>
  <body>
    <script type="text/javascript">

    console.log(f);//undefined
    var f = 100;
    /*
    变量声明提升:变量的定义在执行时会被提升到当前作用域的顶部，但是赋值的操作位置不变
    */
    </script>
  </body>
</html>
```

### 函数声明提升

函数声明提升：会将函数的声明提升到当前作用域的顶部.    

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>作用域</title>
  </head>
  <body>

    <script type="text/javascript">
    fun();//100

    // 函数声明提升：会将函数的声明提升到当前作用域的顶部
    function fun(){
      var f = 100;
      console.log(f);
    }

    </script>

  </body>
</html>
```

### 经典面试题

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>提升</title>
  </head>
  <body>

    <script type="text/javascript">

    var a = 10;

    function fun(){
      console.log(a);//undefined
      var a = 100;

      /*等价代码*/
      /*
      var a;
      console.log(a);//undefined
      a = 100;
      */
    }

    fun();//

    </script>

  </body>
</html>
```

这个题,局部可以调用全局变量,所以应该会显示10.这是错的.不要忘了后面还有个`var a = 100;`,变量声明提升,所以`var a;`被提升到了局部的最顶部,所以全局变量a在局部被局部变量a覆盖了,由于变量声明提升,只有声明,没有赋值,所以这里的a为`undefined`.

## 数值转换

有3个函数可以把非数值转换为数值:`Number()`,`parseInt()`和`parseFloat()`.   

### Number()

`Number()`可以用于任何数据类型,若该类型不能被装换为数字,则返回`NaN`.       
实例:

```
var num1 = Number("Hello world!"); //NaN
var num2 = Number("");//0
var num3 = Number("000011");//11
var num4 = Number(true);//1
var num5 = Number(false);//0
var num6 = Number("123hello");//NaN
var num7 = Number("0x12");//10
```

### parseInt()

`parseInt()`函数在转换字符串时,更多的是看其是否符合数值模式.它会忽略字符串前面的空格,直到找到第一个非空格字符.如果第一个字符不是数字字符或者负号,`parseInt()`就会返回`NaN`;也就是说,用`parseInt()`转换空字符串会返回`NaN`(`Number()`对空字符串返回的是0).    
如果第一个字符是数字字符,`parseInt()`会继续解析第二个字符,直到解析完所有后续字符或者遇到了一个非数字字符.     
例如:`123hello`会被转换为`123`,因为`hello`会被完全忽略.类似的,`22.5`会被转换为`22`,因为小数点并不是有效的数字字符.  

```
console.log(parseInt(""));//NaN
console.log(parseInt("1234abc"));//1234
console.log(parseInt("abc"));//NaN
console.log(parseInt("10.123"));//10
console.log(parseInt("0x12"));//18
console.log(parseInt("070"));//70, 并没有解析为８进制
```

为了消除在使用`parseInt()`函数时可能导致的进制转换问题,可以为这个函数提供第二个参数,转换时使用的基数(即多少进制).也就是说这个只能由别的进制转换为10进制,而不能将10进制转换为其他的进制.

```
console.log(parseInt("010", 8));
console.log(parseInt("10", 2));
console.log(parseInt("20",2));//超过范围时，转换为NaN
console.log(parseInt("10", 10));
console.log(parseInt("10", 16));
```

### parseFloat()

`parseFloat()`函数也是从第一个字符开始的(位置0)开始解析每个字符,都按照十进制解析.而且也是一直解析到字符串的末尾,或者解析遇见一个无效的浮点数字字符为止.      
也就是说,字符串中的第一个小数点是有效的,而第二个小数点就是无效的了,因此他后面的字符串将被忽略.

```
console.log("----parseFloat---------");
console.log(parseFloat("10.234")); //10.234
console.log(parseFloat("10.abc"));//10
console.log(parseFloat("10.23.4"));//10.23
console.log(parseFloat("0x10.234"));//0
console.log(parseFloat("010.234"));//10.234
console.log(parseFloat("abc"));//NaN
```

## 操作符以及控制语句

### break和continue

`break`和`continue`语句用于在循环中精确的控制代码的执行.其中,`break`语句会立即退出循环,强制继续执行循环后面的语句.而`continue`语句虽然也是立即退出循环,但退出循环后会从循环的顶部继续执行.

> `break`只会跳出一层循环.`continue`后面的的语句不会执行.

```
for(var i = 0; i < 10; i++){
    if (i == 5){
       break;
     }
    alert(i);
  }

  for (var i = 0; i < 5; i++){
    if (i < 5){
      continue;
    }
    alert(i);
  }
```

如果是多层循环,你想用`break`跳出所有的循环,就必须用到`label`语句,使用`label`语句可以在代码中添加标签,以便将来使用.

实例:

```
out: for (var j = 0; j < 10; j++) {

    console.log("j = " + j);

    for (var i = 0; i < 10; i++) {
      console.log("i = " + i);
      if (i == 5) {
        //跳出当前循环，继续运行循环后边的代码
        // break;

        //跳出指定循环，继续运行循环后边的代码
        break out;
      }
    }
  }
```

在这里`break`跳出的就不仅仅是一层循环了,而是跳出了`label`语句标记的循环.

### switch语句

`switch`语句与if语句的关系最为密切,而且也是在其他语言中普遍使用的一种流控制语句.    
`switch`语句中使用任何数据类型(在很多其他语言中只能使用数值),无论是字符串,还是对象都没有问题.其次,每个`case`的值不一定是常量,可以是变量,甚至可以是表达式.      

```
switch(expression) {
  case value1:
       statement
       break;
   case value2:
       statement
       break;
   default:
       statement
       break;
}
```

> 注意要使用关键字`break`,否则会导致执行多个`case`语句.不过有些特殊情况也需要忽略`break`以实现某个目标.
没有`break`会导致,匹配到一个`case`,他后面的`case`也会依次执行.

实例:是一个输入日期,求该该天是今年的第多少天?

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>

    <script type="text/javascript">
    var year = 2017, mon = 10, day = 10;
    var leap = 0, sum = 0;

    if ((year % 400 == 0) || ((year % 4 == 0) && (year % 100 != 0))){
      leap = 1;
    }

    switch (mon){
      case  12:
      sum += 30;
      case  11:
      sum += 31;
      case  10:
      sum += 30;
      case  9:
      sum += 31;
      case  8:
      sum += 31;
      case  7:
      sum += 30;
      case  6:
      sum += 31;
      case  5:
      sum += 30;
      case  4:
      sum += 31;
      case  3:
      sum += 28 + leap;
      case  2:
      sum += 31;
      case  1:
      sum += day;
    }

    console.log(sum);
    </script>

  </body>
</html>
```

### JAVAScript函数

函数对于任何编程语言来说都是一个核心的概念.通过函数可以封装任意多条语句,而且可以在任何地方.任何时候调用执行.

```
javascript函数示例:
function sum(arg1,arg2){
  return arg1 + arg2;
}
sum(1,2)
```

在上面的代码中,其中`arg1`,`arg2`为传递的形参,`sum(1,2)`中的数值为实参.

#### arguments对象

`arguments`是存储了函数传递过来的实参的,并且arguments对象只有函数开始时才可用.`arguments`对象只是与数组类似(它并不是ARRAY的实例),因为可以使用方括号语法访问它的每一个元素(即第一个元素是`arguments[0]`,第二个元素是`arguments[1]`,依次类推),使用`length`属性来确定传递进来多少个参数.     

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>函数传递参数</title>
  </head>
  <body>

    <script type="text/javascript">

//参数个数随意
     //arguments 是一个伪数组
     function sum(arg){

      //arguments只能在函数内部使用！！！！！
      //arguments 用于获得实参
       console.log(arguments);

       var result = 0;

       //arguments.length是参数的个数
       for (var i = 0; i < arguments.length; i++) {
         //arguments[i], 就指定参数
         result += arguments[i];
       }
       console.log(result);
     }

     console.log(sum.length);//1
     //该函数的形式参数个数！！！！！！

     sum();
     sum(1);
     sum(1,2);
     sum(1,2,3);

         </script>

       </body>
     </html>

```

#### 函数返回值

函数调用一次之后表示运算完毕,使用return关键字可以返回结果,同样return可以提前结束函数的运行.

```
function fun(arg1,arg2){
  return arg1 + arg2;
  console.log("after return ...");//不会执行
}
fun();//返回计算结果

function fun1(arg1){
  console.log(arg1);
  return;//返回undefined
}
```

### 布尔操作符

布尔操作符一共有3个:与,非,或.

#### 逻辑非(!)

```
alert(!false); //true
alert(!"blue");//false
alert(!0);//true
alert(!NaN);//true
alert(!"");//true
alert(!12345);//false
```

#### 逻辑与(&&)

操作符由两个和号(&&)表示,至少有两个操作数.      
逻辑与的特点：短路     
短路：与运算从左向右执行，一旦遇到一个为假的,那么后边的不再执行      

```
console.log(10 && 0);//0
console.log(0 && 10);//0
console.log(10 && 20);//20
console.log(10 && true);//true
console.log(10 && false);//false

var d = 10;

//d++本身是一个表达式，只不过该表达式的值等于 没有自增1之前的值。
//++d本身也是一个表达式,只不过该表达式的值等于自增1以后的值.
console.log(10 && 20 && 30 && d++);//10
console.log(d);//11

```

#### 逻辑或(||)

操作符由两个竖线符号(||)表示,至少有两个操作数.    
它的特点和逻辑与类似,也是短路,不过他是从左向右执行,一旦遇到一个为真,那么后面的不在执行.   

```
console.log(10 || false);//10
console.log(false || 10 || 20 );//10
```

### 关系操作符

小于(<),大于(>),小于等于(<=),和大于等于(>=),相等(==)，不等(!=)，全等(===)，不全等(!==)操作符.

1. 如果两个操作数都是数值,则执行数值比较.
2. 如果两个操作数都是字符串,则比较两个字符对应的字符编码值.
3. 如果一个操作数是数值,则将另一个操作数转换为一个数值,然后执行数值比较.
4. 如果一个操作数是对象,则调用这个对象的`valueOf()`方法,用得到的结果按照前面的规则执行比较.如果对象没有`valueOf()`方法,则调用`toString()`方法,并用得到的结果根据前面的规则执行比较.
5. 如果一个操作数是布尔值,则现将其转换为数值,然后再执行比较.

```
console.log(10 > 20);//false
console.log(10 < 20);//true
console.log(10 < "23");//true
console.log("23" > "233");//false,按照编码的大小去比较,它是挨个字符去比较.

console.log(10 > NaN);//false
console.log(10 < NaN);//false
console.log(10 == NaN);//false
console.log(NaN == NaN);//false


/*
==  相等, 是先转换再比较
=== 全等, 是仅比较而不转换
*/

console.log(10 == "10");//true
console.log(10 === "10");//false

console.log(10 != "10");//false
console.log(10 !== "10");//true

```

### 三目运算符

`var result = 表达式１? 表达式2: 表达式3;`      
如果表达式１的值为true, 那么表达式2的值会赋值给result;否则，表达式3的值会赋值给result.

```

// 如果表达式１的值为true, 那么表达式2的值会赋值给result;否则，表达式3的值会赋值给result
    var result = 10 > 20 ?  'hello' : 'world';
    console.log(result);//world

    /*
    if (10>20){
    result = 'hello';
    } else {
    result = 'world';
    }
    */
```

### 复合操作符

复合操作符用于简化操作符,提升性能.

```
//复合运算符
var a = 10, b = 20;
a += b; // a = a + b;
a -= b; // a = a - b;
a *= b; // a = a * b;
a /= b; // a = a / b;
a %= b; // a = a % b;
```

### 逗号操作符

使用逗号操作符可以在一条语句中执行多个操作,如下所示:     
`var num = 1, num1 = 2;`      

逗号操作符还可以用于赋值.在用于赋值时,逗号操作符总会返回表达式中的最后一项,比如:    
`var result = (表达式1,表达式2,表达式3);`    
result的值为表达式3的值.逗号运算符取最后一个的结果为整个表达式的结果,但是前边所有的表达式都会运算.

## JAVAScript数组

数组是值的有序集合,每个值就是一个元素,每个元素在数组中有一个位置,以数字表示,成为索引.JAVAScript数组是无类型,数组元素可以是任意类型,并且同一个数组中的不同元素也可能是不同的类型.

```
var  one = [];
console.log(typeof one);//object
```

### 创建数组

使用数组直接量进行创建,方括号中使用逗号隔开:

```
var a = [];
var b = [10,2,3];
var c = [1.1,true,'a'];
var d = [1,2,,3];  //其中省略的元素值为undefined,稀疏数组
```

使用Array()创建:

```
var a = new Array();//创建空数组
var b = new Array(10);//创建长度为10的元素
var c = new Array(1,2,3);//创建一个已经包含三个元素的数组
```

### 数组的读写操作:

```
var a = ["hello"];
var b = a[0];
a[1] = "world";
i = 1;
a[i + 2] = "string";
```

### 数组的长度:

```
var a = [1,2,3,4];
console.log(a.length); //长度为４

a[999] = 999; //长度为　1000, 稀疏数组的长度大于元素的个数
console.log(a.length);// 长度为1000
```

### 数组的遍历

使用for循环是最常见的遍历数组的方式:

```
var a = [1,2,3,4,5,6];
for(var i = 0; i < a.length; i++){
  console.log(a[i]);
}
//这里还是可以提高效率的!如下:
for(var i = 0, len = a.length; i < len; i++){//这里把长度赋值,不用每次去调用这个方法.
  console.log(a[i]);
}
```

更简单的方式:

```
for (var i in a){
  console.log(a[i]);
}
```
使用一个方法`forEach()`也是可以遍历的,后面会说到.

## 数组常用的方法

### 数组元素的添加和删除

添加:

- 直接对新索引值进行赋值(不是一个方法).
- `push()`使用`push()`方法在数组末尾添加一个或者多个元素.返回值为新长度.
- `unshift()`使用`unshift()`方法在数组的开端添加一个或者多个元素.

删除:

- 直接用delete删除,如:`delete a[0];`,他不会改变元素的索引位置和长度,只是把那个元素变为`undefined`.
- `pop()`删除最后一个值,长度减一.返回值为删除的那个元素.
- `shift()`方法从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>数组的添加和删除</title>
  </head>
  <body>

    <script>
    // 直接对新索引值进行赋值
    var a1 = [1,2,3];
    a1[3] = 4;
    a1[4] = 5;

    //直接用delete删除
    delete a1[0];//不影响长度

    //末尾的添加，删除
    console.log(a1.push(10,20,30));//8
    console.log(a1);//[undefined,2,3,4,5,10,20,30]

    //删除最后一个，长度减1
    console.log(a1.pop());//30

    //开头添加，删除
    //shift unshift
    var a2 = [1,2,3,4,5];
    console.log(a2.shift());//1

    // a2.unshift(10,20,30);
    a2.unshift(10);
    a2.unshift(20);
    a2.unshift(30);
    console.log(a2);//[30,20,10,2,3,4,5]
    </script>

  </body>
</html>
```

### join()方法

`join()`方法,数组拼接,不会改变原有数组,只是将拼接的字符串作为返回值,默认用`,`拼接.

```
var a2 = [1,2,3,4,5,6];
//join 数组拼接
console.log(a2.join());//1,2,3,4,5,6
console.log(a2.join(''));//123456
console.log(a2.join('*'));//1*2*3*4*5*6
console.log(a2.join('+'));//1+2+3+4+5+6
console.log(a2);//[1,2,3,4,5,6],可见原有数组并不会发生改变
```

### reverse()方法

`reverse()`颠倒方法,将数组中元素的位置颠倒。第一个数组元素成为最后一个数组元素，最后一个数组元素成为第一个。reverse 方法颠倒数组中元素的位置，并返回该数组的引用。

```
//reverse颠倒，数组本身也被修改了
var a4 = ['h', 'e', 'l', 'l', 'o'];
console.log(a4.reverse());//['o', 'l', 'l', 'e', 'h']
console.log(a4.reverse());//['h', 'e', 'l', 'l', 'o']
```

### sort()方法,对数组进行排序

`sort()`方法在适当的位置对数组的元素进行排序，并返回数组。 sort 排序不一定是稳定的。默认排序顺序是根据字符串Unicode码点。   
参数为一个函数,如果没有参数,默认按`unicode`码进行排序.

```
var a9 = [10,2,3,80];
    console.log(a9.sort());//按照字符的编码进行排序，默认从小到大,比较的是第一个字符.

    // sort传入参数进行排序
    a9.sort(function(a,b){
      console.log("a = %d, b = %d", a, b);

      //从小到大排序为a-b,从大到小排序为b-a
      return a - b;
    });

    //影响原数组
    console.log(a9);
```

### concat方法，连接数组并返回一个新数组

`concat() `方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```
var num1 = [1, 2, 3];
var num2 = [4, 5, 6];
var num3 = [7, 8, 9];
// 组成新数组[1, 2, 3, 4, 5, 6, 7, 8, 9]; 原数组 num1, num2, num3 未被修改
var nums = num1.concat(num2, num3);



var alpha = ["a", "b", "c"];
var numeric = [1, 2, 3];
// 组成新数组 ["a", "b", "c", 1, 2, 3]; 原数组 alpha 和 numeric 未被修改
var alphaNumeric = alpha.concat(numeric);
```

### slice 方法，对数组内容进行浅复制

`slice()`方法返回一份从开始到结束(不包括结束)选择的数组的一部分浅拷贝到一个新数组对象.原始数组不会被改变.     
两个参数:分别为开始位置和结束位置.都是可选参数.返回值为一个含有提取元素的新数组.

```
//slice 数组切片
   var a12 = [1,2,3,4,5,6,7,8,9,10];

   //begin end(不包含)
   console.log(a12.slice(1,4));//[2,3,4]
   console.log(a12.slice());//全切
   console.log(a12.slice(3));//从3开始切，到最后
   console.log(a12.slice(-7,8));//从4开始。。。。。

   var result = a12.slice(0,3);
   result[0] = 100;
   //result, a12不是同一块内存地址，相互不影响
   console.log(a12, result);



   //slice浅复制(引用类型)
   var a13 = [[100,200,300],1,2,3,4,5];
   result = a13.slice(0,3);

   //修改切片后的数组
   result[0][0] = 1000;
   console.log(a13, result);//互相影响,引用的是同一块内存地址.
```

### splice　对数组元素进行替换修改

splice() 方法通过删除现有元素和/或添加新元素来更改一个数组的内容。   

语法:

- array.splice(start)
- array.splice(start, deleteCount)
- array.splice(start, deleteCount, item1, item2, ...)

参数:

- start​:指定修改的开始位置（从0计数）。如果超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从1计数）。
- deleteCount 可选 :整数，表示要移除的数组元素的个数。如果 deleteCount 是 0，则不移除元素。这种情况下，至少应添加一个新元素。如果 deleteCount 大于start 之后的元素的总数，则从 start 后面的元素都将被删除（含第 start 位）。如果deleteCount被省略，则其相当于(arr.length - start)。
- item1, item2, ... 可选:要添加进数组的元素,从start 位置开始。如果不指定，则 splice() 将只删除数组元素。

返回值:

由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。

```
/*
splice: 添加，删除，修改
*/
var myFish = ["angel", "clown", "mandarin", "surgeon"];
result = myFish.splice(0,1);//result，删除元素组成的数组
console.log(myFish,result);//['angel']

//任意位置添加
myFish.splice(1,0,"hello", "world");
console.log(myFish);//["clown","hello", "world","mandarin", "surgeon"]

//修改
myFish.splice(2,1, "china");
console.log(myFish);//["clown","hello", "china","mandarin", "surgeon"]
```

### toString()

`toString()` 返回一个字符串，表示指定的数组及其元素。与`join()`类似.但是他没有参数,只能以`,`连接.

```
var monthNames = ['Jan', 'Feb', 'Mar', 'Apr'];
var myVar = monthNames.toString(); // assigns "Jan,Feb,Mar,Apr" to myVar.
```

### ECMA5中数组新方法

#### array.forEach()

`array.forEach(callback[, thisArg])`

callback 在数组每一项上执行的函数，接收三个参数：

- currentValue, 当前项（指遍历时正在被处理那个数组项）的值。
- index, 当前项的索引（或下标）。
- array, 数组本身。
- thisArg 可选参数。用来当作callback 函数内this的值的对象.

>  forEach在遍历完数组元素之前是不会终止的.

```
// forEach直接遍历
var a14 = [1,2,3,4,5,6,7,8];
var sum = 0;

// 直接遍历
a14.forEach(function(current,index, arr){
  console.log("current = ", current);
  // console.log("index = ", index);
  // console.log("arr = ", arr);
  sum += current;
});
console.log(sum);
```

#### array.map()

`array.map(callback[, thisArg])`

callback 在数组每一项上执行的函数，接收三个参数：

- currentValue, 当前项（指遍历时正在被处理那个数组项）的值。
- index, 当前项的索引（或下标）。
- array, 数组本身。

thisArg 可选参数。用来当作callback 函数内this的值的对象。

返回值： 一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组

```
// for (var i = 0; i < a14.length; i++) {
//   a14[i] += 10;
// }

//map 等价于上面的函数
var result = a14.map(function(current,index,arr){

  //map需要每次返回，由返回的数据共同组成一个新数组
   return current + 10;
});
console.log(result);
```

#### indexOf()

`arr.indexOf(searchElement[, fromIndex = 0]) `

- searchElement   要查找的元素        
- fromIndex     指定开始位置,默认为0  

返回给定元素能找在数组中找到的第一个索引值，否则返回-1。  

```
var a15 = [10,20,30,20,80,60,34];
console.log(a15.indexOf(20));//1
console.log(a15.indexOf(200));//-1
```

#### filter()

`arr.filter(callback[, thisArg])`

callback 用来测试数组的每个元素的函数。调用时使用参数 (element, index, array)。

- element, 当前项（指遍历时正在被处理那个数组项）的值。
- index, 当前项的索引（或下标）。
- array, 数组本身。

返回true表示保留该元素（通过测试），false则不保留。该方法的返回值为通过测试的元素组成的新的数组.
thisArg 可选。执行 callback 时的用于 this 的值。

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>filter方法</title>
  </head>
  <body>

    <script>
      var a = [1,2,3,4,5,6,7,8,9,10];

      var b = a.filter(function(element,index,array){
        return element % 2 === 0;//其返回值为true的元素,都被添加到了其返回值中.
        // 上面的代码等价于下面的
        /*
         if(element%2===0){
          return true;
         }else{
          return false;
         }
         */
      })

      console.log(a);//[1,2,3,4,5,6,7,8,9,10],可见这个方法对原数组不会产生影响.
      console.log(b);//[2,4,6,8,10]

      //如何将一个稀疏数组变为稠密数组?
      var x = [1,,,,,,,,4,5,6,,,,7];

      var y = x.filter(function(element,index,array){
        return element === undefined ? false : true ;//三目运算
      })
      console.log(y);
    </script>

  </body>
</html>
```

#### every()和some()

`array.every(callback[,thisArg])`,`array.some(callback[,thisArg])`

callback 用来测试数组的每个元素的函数。调用时使用参数 (element, index, array)。   
thisArg 可选。执行 callback 时的用于 this 的值。

不同点:     

- `array.every()`只有所有元素都通过函数测试，才会返回true,否则返回false.
- `array.some()`只要其中一个元素通过了函数测试,就会返回true,全部不通过才会返回false.

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>every some</title>
  </head>
  <body>
    <script type="text/javascript">
      var a = [2,4,6,8,10,12,14];
      console.log(a.every(function(element,index,array){
        return element%2===0;
      }));//true

      var b = [2,4,6,8,10,12,13];
      console.log(b.every(function(element,index,array){
        return element%2===0;
      }));//false

      var a = [2,3,5,7,9,11,13];
      console.log(a.some(function(element,index,array){
        return element%2===0;
      }));//true

      var a = [1,3,5,7,9,11,13];
      console.log(a.some(function(element,index,array){
        return element%2===0;
      }));//false

    </script>

  </body>
</html>
```

#### reduce()

`arr.reduce(callback,[initialValue])`              
callback  执行数组中每个值的函数，包含四个参数:

- previousValue   上一次调用回调返回的值，或者是提供的初始值（initialValue）        
- currentValue   数组中当前被处理的元素          
- index   当前元素在数组中的索引         
- array   调用 reduce 的数组

initialValue  作为第一次调用 callback 的第一个参数。   
返回一个值。  

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>reduce</title>
  </head>
  <body>
    <script type="text/javascript">
      var a = [1,2,3,4,5,6,7,8,9,10];

      var b = a.reduce(function(initvalue,element,index,array){
        return initvalue + element;//在不提供初始值的情况下,initvalue从数组的第一个值开始.
        //本次的返回值会赋给initvalue
      })
      console.log(b);//55

      //当然你也可以设置初始值,如下:

      var c = a.reduce(function(initvalue,element,index,array){
        return initvalue * element;
      },1)//在这里设置初始值
      console.log(c);//3628800
    </script>

  </body>
</html>
```

## JAVAScript

JAVAScript与HTML之间的的交互是通过事件实现的.事件就是文档或浏览器窗口中发生的一些特定的交互瞬间.可以使用侦听器(或处理程序)来预定事件,以便事件发生时执行相应的代码.   
JAVAScript事件的三要素:事件源, 事件, 事件处理程序三部分组成.

- 事件源,指的是发生事件的对象
- 事件,指的是做了什么动作
- 事件处理程序,发生事件时要做的事

### 事件流

如果你单击了某个按钮,他们会认为不仅仅发生在按钮上.换句话说,在单击按钮的同时,你也单击了按钮的容器元素,甚至你也单击了整个页面.事件流描述的是从页面中接受时间的顺序.有意思的是ie和netspace开发团队提出了差不多是完全相反的事件流的概念.ie的事件流是事件冒泡流,而netspace是事件捕获流.

![](http://oujvmc3la.bkt.clouddn.com/dom.png)

#### 事件冒泡

ie的事件流叫做事件冒泡(event bubbling),即时间开始时由最具体的元素(文档中嵌套层次最深的那个节点)接受,然后逐级向上传播到较为不具体的节点(文档).

```
<!DOCTYPE html>
<html>
<head>
<title>Event Bubbling Example</title>
</head>
<body>
<div id="myDiv">Click Me</div>
</body>
</html>
```

如果你单击了页面中的`<div>`元素,那么这个click事件会按照如下顺序传播:

1. `<div>`
2. `<body>`
3. `<html>`
4. `document`

也就是说,click事件首先是在`<div>`元素上发生,而这个元素就是我们单击的元素.然后,click事件沿DOM树向上传播,在每一级节点上都会发生,直至传播到`document`对象.

#### 事件捕获

事件捕获的思想是不太具体的节点应该更早的接收到事件,而更具体的节点应该最后接收到事件.     
如果仍以前面的HTML页面作为演示事件捕获的例子,那么单击`<div>`元素就会以下列顺序触发click事件.

1. `document`
2. `<html>`
3. `<body>`
4. `<div>`

在事件捕获过程中,document对象首先接收到click事件,然后事件沿DOM树依次向下,一直传播到时间的实际位置,即`<div>`元素.

> 由于老版本的浏览器不支持,因此很少有人使用事件捕获.不过我们可以放心的使用事件冒泡.

#### DOM事件流

DOM2级事件”规定的事件流包括三个阶段:事件捕获阶段、处于目标阶段和事件冒泡阶段。首
先发生的是事件捕获,为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶
段,可以在这个阶段对事件做出响应。

### 事件处理程序

事件就是用户或浏览器自身执行的某种动作。诸如 click 、 load 和 mouseover ,都是事件的名字。而响应某个事件的函数就叫做事件处理程序(或事件侦听器)。事件处理程序的名字以 "on" 开头,因此click 事件的事件处理程序就是 onclick , load 事件的事件处理程序就是 onload 。为事件指定处理程序的方式有好几种。

#### HTML事件处理程序

某个元素支持的每种事件,都可以使用一个与相应事件处理程序同名的 HTML 特性来指定。这个
特性的值应该是能够执行的 JavaScript 代码。例如,要在按钮被单击时执行一些 JavaScript,可以像下面
这样编写代码:

```
<input type="button" value="Click Me" onclick="alert('Clicked')" />
```

当单击这个按钮时,就会显示一个警告框。这个操作是通过指定 onclick 特性并将一些 JavaScript
代码作为它的值来定义的。

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>原生事件event</title>
    <style>
      * {
        margin: 0;
        padding: 0;
      }

      .one {
        width: 200px;
        height: 200px;
        background: blue;
      }
    </style>
  </head>
  <body>
    <script type="text/javascript">
    function clickDiv(){
      alert("我被点了。。。");
    }
    </script>

    <!-- html中添加事件  -->
    <button id="btn" onclick="alert('我被点了。。。')">点我</button>
    <div class="one" onclick="clickDiv()"></div>

  </body>
</html>
```

#### DOM0级事件处理程序

通过 JavaScript 指定事件处理程序的传统方式,就是将一个函数赋值给一个事件处理程序属性。这种为事件处理程序赋值的方法是在第四代 Web 浏览器中出现的,而且至今仍然为所有现代浏览器所支持。原因一是简单,二是具有跨浏览器的优势。要使用 JavaScript 指定事件处理程序,首先必须取得一个要操作的对象的引用。

每个元素(包括 window 和 document )都有自己的事件处理程序属性,这些属性通常全部小写,
例如 onclick 。将这种属性的值设置为一个函数,就可以指定事件处理程序,如下所示:

```
var btn = document.getElementById("myBtn");
btn.onclick = function(){
alert("Clicked");
};
```

也可以删除通过 DOM0 级方法指定的事件处理程序,只要像下面这样将事件处理程序属性的值设
置为 null 即可:

```
btn.onclick = null;//删除事件处理程序
```

将事件处理程序设置为 null 之后,再单击按钮将不会有任何动作发生。

实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>event事件注册</title>
  </head>
  <body>
    <button id="btn1">按钮1</button>
    <button id="btn2">按钮2</button>

    <script type="text/javascript">

    //通过document.getElementById方法得到DOM对象
    var btn1 = document.getElementById("btn1");
    var btn2 = document.getElementById("btn2");

    //使用DOM0级的方式注册一个事件处理程序
    /*
    缺点：
    1. 同一个事件，只能注册一个事件处理程序！！！！
    2. 移除一个事件程序，不方便
    */
    btn1.onclick = function () {
      console.log("%c按钮1被点击了xxxxx", "color: red;font-size:50px");

      // 移除一个事件处理程序
      btn1.onclick = null;
    };


    btn2.onclick = function () {
      console.log("%c按钮2被单击了", "color: red;font-size:50px");
    };

    btn2.ondblclick = function () {
      console.log("%c按钮2被双击了", "color: green;font-size:50px");
    };


    </script>

  </body>
</html>
```

#### DOM2级事件处理程序

“DOM2 级事件”定义了两个方法,用于处理指定和删除事件处理程序的操作: `addEventListener()`
和 `removeEventListener()` 。所有 DOM 节点中都包含这两个方法,并且它们都接受 3 个参数:要处
理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值参数如果是 true ,表示在捕获
阶段调用事件处理程序;如果是 false ,表示在冒泡阶段调用事件处理程序。
要在按钮上为 click 事件添加事件处理程序,可以使用下列代码:

```
var btn = document.getElementById("myBtn");
btn.addEventListener("click", function(){
alert(this.id);
}, false);
```

上面的代码为一个按钮添加了 onclick 事件处理程序,而且该事件会在冒泡阶段被触发(因为最
后一个参数是 false )。与 DOM0 级方法一样,这里添加的事件处理程序也是在其依附的元素的作用域
中运行。使用 DOM2 级方法添加事件处理程序的主要好处是可以添加多个事件处理程序。来看下面的
例子。

```
var btn = document.getElementById("myBtn");
btn.addEventListener("click", function(){
alert(this.id);
}, false);
btn.addEventListener("click", function(){
alert("Hello world!");
}, false);
```

这里不推荐使用匿名函数来添加事件,会导致`removeEventListener()`,因为传入`removeEnventListener()`的事件处理函数必须和`addEnventListener()`一致.所以应该如下所示:

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>DOM2级event</title>
</head>

<body>

  <button id="btn">按钮</button>

  <script type="text/javascript">
    var btn = document.getElementById("btn");

    function handler1() {
      console.log("%c 单击111", "color:red;font-size:40px");
    }

    function handler2() {
      console.log("%c 单击222", "color:red;font-size:40px");

      // 移除click事件的第一个事件处理程序
      // 要想移除处理函数，那么该函数必须有名字(在指定的作用域中可访问到)
      btn.removeEventListener('click', handler1, false);
      btn.removeEventListener('click', handler2, false);
    }

    btn.addEventListener('click', handler1, false);
    btn.addEventListener('click', handler2, false);
  </script>

</body>

</html>
```

> DOM2级的事件执行顺序和注册顺序相同.

#### ie的事件处理程序

IE 实现了与 DOM 中类似的两个方法: `attachEvent()` 和 `detachEvent()` 。这两个方法接受相同的两个参数:事件处理程序名称与事件处理程序函数。由于 IE8 及更早版本只支持事件冒泡,所以通过`attachEvent() `添加的事件处理程序都会被添加到冒泡阶段。
要使用 `attachEvent()` 为按钮添加一个事件处理程序,可以使用以下代码。

```
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
alert("Clicked");
});
```

> 注意: `attachEvent()`中的第一个参数为`onclick`,而不是DOM二级中的`click`.
还有在同一个元素上注册相同的事件,其执行顺序和注册顺序相反,这也是和DOM2级不同的.
DOM0级被认为是元素的方法,所以其this引用当前的元素;  IE事件处理程序的this等于window.

使用 `attachEvent()` 添加的事件可以通过 `detachEvent()` 来移除,条件是必须提供相同的参数。
与 DOM 方法一样,这也意味着添加的匿名函数将不能被移除。不过,只要能够将对相同函数的引用传
给 `detachEvent()` ,就可以移除相应的事件处理程序。例如:

```
var btn = document.getElementById("myBtn");
var handler = function(){
alert("Clicked");
};
btn.attachEvent("onclick", handler);
//这里省略了其他代码
btn.detachEvent("onclick", handler);
```

这个例子将保存在变量 handler 中的函数作为事件处理程序。因此,后面的 `detachEvent()` 可以
使用相同的函数来移除事件处理程序

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>DOM2级event</title>
</head>
<body>

  <button id="btn">按钮</button>

  <script type="text/javascript">
    var btn = document.getElementById("btn");

    function handler1() {
      console.log("%c 单击111", "color:red;font-size:40px");
      btn.detachEvent('onclick', handler2);
    }

    function handler2() {
      console.log("%c 单击222", "color:red;font-size:40px");
    }

    /*
    ie： DOM2级的事件处理
    ie: 处理程序的执行顺序是和注册顺序相反的
    */
    btn.attachEvent('onclick', handler1);
    btn.attachEvent('onclick', handler2);

    document.attachEvent('onclick', function(){
      console.log("doucment被触发。。。。");
    });


  </script>

</body>

</html>
```

#### 跨浏览器的事件处理程序

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title></title>
</head>

<body>

  <button id="btn">btn</button>
  <button type="button" name="button" id="btn1"></button>

  <script type="text/javascript">
    /*
        功能：兼容浏览器的事件注册函数
        @elem, 指定要注册的DOM对象
        @type, 指定要注册的事件名称
        @handler, 指定的事件处理函数
        */

    function addEvent(elem, type, handler) {

      //判断是否支持该方法，支持就为true，不支持就为false
      if (elem.addEventListener) {
        elem.addEventListener(type, handler, false);
      } else if (elem.attachEvent) {
        elem.attachEvent('on' + type, handler);
      } else {
        elem['on' + type] = handler;
      }
    }

    var btn = document.getElementById("btn");
    var deal = function() {
      console.log("btn 点击。。。。。");
    }
    addEvent(btn, 'click', deal);

    addEvent(btn, 'dblclick', function() {
      console.log("btn 双击。。。。。");
    });


    // 编写 removeEvent来移除事件！！！！！
    function removeEvent(elem, type, handler) {
      if (elem.removeEventListener) {
        elem.removeEventListener(type, handler, false);
      } else if (elem.detachEvent) {
        elem.detachEvent('on' + type, handler);
      } else {
        elem['on' + type] = null;
      }
    }

    removeEvent(btn, 'click', deal)
  </script>

</body>

</html>
```

### 事件对象

在触发DOM上的某个事件时,会产生一个事件对象event,这个对象中包含着所有与事件有关的信息.包括导致事件的元素,事件的类型以及其他与特定事件相关的信息.例如,鼠标操作导致的事件对象中,会包括鼠标的位置,而键盘操作导致的事件对象中,会包含与按下键的信息.所有浏览器都支持event对象,但支持方式不同.

#### DOM中的事件对象

兼容DOM的浏览器都会将一个event对象传入到事件处理程序中.无论指定事件处理程序使用的是什么方法(DOM0级或DOM2级),都会传入event对象.来看下面的例子:

```
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
alert(event.type);//"click"
};
btn.addEventListener("click", function(event){
alert(event.type);//"click"
}, false);
```

这个例子的两个事件处理程序都会弹出一个警告框,显示由event.type属性表示的事件类型.这个属性始终都会包含被触发的事件类型.

|------|-------|
|属性|说明|
|event.type|被触发的事件类型|
|this|注册该事件处理程序的元素|
|event.currentTarget|注册该事件处理函数的元素|
|event.target|触发该事件源头的元素|
|event.stopPropagation()|阻止事件的传递|
|event.preventDefault()|阻止默认行为|
|event.stopImmediatePropagation()|停止冒泡,并且停止后续所有的事件处理程序|

键盘事件对象

|------|-------|
|event.key|具体按下哪个键,识别大小写|
|event.code|按下的是哪个键,不识别大小写|

鼠标事件对象

|---------|---------|
|event.screenX|点击时鼠标处于屏幕的X轴位置|
|event.screenY|点击时鼠标处于屏幕的Y轴的位置|
|event.clientX|点击时鼠标处于浏览器可视窗口的X轴位置|
|event.clientY|点击时鼠标处于浏览器可视窗口的Y轴位置|
|event.pageX|点击时鼠标处于页面的X轴位置|
|event.pageY|点击时鼠标处于页面的Y轴位置|
|event.which|鼠标点击的是哪个键,左(1),中(2),右(3)|
|event.button|鼠标点击的是哪个键,左(0),中(1),右(2)|

手机触摸

|--------|-------|-------|
|事件|touchstart|手指放上去|
|事件|touchmove|手指移动|
|事件|touchend|手指离开|

手机触摸实例:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>touch</title>
  </head>
  <body>
    <script type="text/javascript">

    var startX,startY,endX,endY,X,Y;
      document.addEventListener('touchstart',function(event){
         window.startX = event.touches[0].clientX;
         window.startY = event.touches[0].clientY;
      },false);
      document.addEventListener('touchend',function(event){
         window.endX = event.changedTouches[0].clientX;
         window.endY = event.changedTouches[0].clientY;
         X = startX - endX;
         Y = startY - endY;
         console.log(X);
         console.log(Y);
         if(Math.abs(X)>Math.abs(Y)){
           if(X>0){
             alert('向左滑动');
           }
           if(X<0){
             alert('向右滑动');
           };
         };
          if(Math.abs(X)<=Math.abs(Y)){
            if(Y>0){
              alert('向上滑动');
            };
            if(Y<0){
              alert('向下滑动');
            };
          };

      },false);

    </script>

  </body>
</html>

```

#### ie中的事件对象

与访问DOM中的event对象不同,要访问IE中的event对象有几种不同的方式,取决于指定事件处理程序的方法.在使用DOM0级方法添加事件处理程序时,event对象作为window对象的一个属性存在.

```
var btn = document.getElementById("myBtn");
btn.onclick = function(){
var event = window.event;
alert(event.type);
//"click"
};
```

在此,我们通过window.event取得了event对象,并检测了被触发事件的类型(IE中的type属性与DOM中的type属性是相同的).可是,如果事件处理程序是使用`attachEvent()`添加的,那么就会有一个event对象作为参数被传入事件处理程序函数中,如下所示:

```
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(event){
alert(event.type);
//"click"
});
```

在像这样使用`attachEvent()`的情况下,也可以通过window对象来访问event对象,就像使用DOM0级方法一样.不过为方便起见,同一个对象也会作为参数传递.

IE的event对象同样也包含与创建它的事件相关的属性和方法.其中很多属性和方法都有对应的或者相关的DOM属性和方法.与DOM的event对象一样,这些属性和方法也会因为事件类型不同而不同,但所有事件对象都会包含下表所类的属性和方法.

|---------|---------|
|cancelBubble|默认值为 false ,但将其设置为 true 就可以取消事件冒泡(与DOM中的 stopPropagation() 方法的作用相同)|
|returnValue|默认值为 true ,但将其设置为 false 就可以取消事件的默认行为(与DOM中的 preventDefault() 方法的作用相同)|
|srcElement|事件的目标(与DOM中的 target 属性相同)|
|type|被触发的事件的类型|


```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>ie中的event对象</title>
  </head>
  <body>

    <button id="btn">按钮</button>
    <a href="https://www.baidu.com" id="ln" target="_blank">超链接</a>

    <script type="text/javascript">
    console.log(window);

    var btn = document.getElementById("btn");
    var ln = document.getElementById("ln");

    btn.onclick = function () {

      //ie低版本中处理函数中，没有event对象
      //所以只能有window.event来代替
      // console.log(window.event);

      console.log(this);
      //ie 低版本中 window.event对象上，没有target,currentTarget
      //使用srcElement取代target


      //停止事件冒泡！！！！
      //cancelBubble 取代 stopPropagation()
      window.event.cancelBubble = true;
    }

    //document 在低版本ie onclick不行
    document.attachEvent('onclick', function(){
      console.log("document.....");

      // window.event.srcElement是触发事件的源头
      console.log(window.event.srcElement);
    },false);


    ln.onclick = function () {

      // window.event.returnValue 的值来决定是否取消默认行为，
      // 和 event.preventDefault() 功能一致
      window.event.returnValue = false;
    }
    </script>


  </body>
</html>

```

>需要注意的是:`this`和`event.srcElement`在DOM0级中是相同的,但是用`attachEvent()`添加的事件处理程序,`this`和`event.srcElement`是不同的.

#### 兼容浏览器的事件对象(基本)

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>兼容浏览器的事件对象(基本)</title>
</head>

<body>


  <button id="btn">按钮</button>

  <script type="text/javascript">
    var btn = document.getElementById("btn");

    btn.onclick = function(event) {
      // 使用或运算或者条件判断来 实现event的兼容
      var event = event || window.event;
      console.log(event.target || event.srcElement);
      event.stopPropagation ? event.stopPropagation() : event.cancelBubblue = true;
    }

  </script>


</body>
</html>
```
## DOM对象模型

DOM为文档提供了结构化表示,并定义了如何通过脚本来访问文档结构.整个文档就是一棵树,树的根是`document`

![](http://oujvmc3la.bkt.clouddn.com/dom.jpg)

### node节点

整个文档全都是由节点构成的,包括元素(HTML标签),文字节点,属性节点.     
每个节点都有一个nodeType属性,用于表明节点的类型.节点类型由在Node类型中定义的下列12个数值常量来表示,任何节点类型必居其一:

- Node.ELEMENT_NODE(1)；标签节点
- Node.ATTRIBUTE_NODE(2)；
- Node.TEXT_NODE(3)；文本节点
- Node.CDATA_SECTION_NODE(4)；
- Node.ENTITY_REFERENCE_NODE(5)；
- Node.ENTITY_NODE(6)；
- Node.PROCESSING_INSTRUCTION_NODE(7)；
- Node.COMMENT_NODE(8)；
- Node.DOCUMENT_NODE(9)；document节点
- Node.DOCUMENT_TYPE_NODE(10)；
- Node.DOCUMENT_FRAGMENT_NODE(11)；
- Node.NOTATION_NODE(12)。

>nodeName可以获得节点名称.

### 获得节点

- document.getElementById(“id”) id 为标记的 #id
- document.getElementsByTagName(“div”) 所有的div div
- document.getElementsByClassName(“test”) 所有类名为 test

id不能指定一个范围去查找元素,但是剩下的两个可以,如下:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    我是body
    <!-- DOM操作，节点操作，元素操作  -->


    <button id="btn">按钮</button>

    <ul id="items">
      <li class="item">1</li>
      <li class="item">2</li>
      <li class="item">3</li>
      <li class="item">4</li>
      <li class="item">5</li>
      <li>6</li>
      <li>7</li>
    </ul>

    <ul>
      <li class="item">11</li>
      <li class="item">22</li>
      <li class="item">33</li>
    </ul>

    <script type="text/javascript">
    //获得某个ul
    var items = document.getElementById("items");
    //getElementsByClassName 可以指定在某个范围内去查找元素
    console.log(items.getElementsByClassName("item"));//5

    // 通过 tagName 标签名
    console.log(document.getElementsByTagName("li"));//10
    console.log(items.getElementsByTagName("li"));//7

    </script>


</body>
</html>

```

> 无论返回多少个节点,返回的都是伪数组,它是能够通过`[0]`位置索引,还有`length`,但是他没有数组的那些方法.

### 节点访问

#### 父节点

`var parent = obj.parentNode;`

#### 兄弟节点

`nextSibling`,`nextElementSibling`都是下一个兄弟,但是`nextSibling`会将文本节点识别为兄弟,而`nextElementSibling`只会将标签识别为兄弟.    
`previousSibling`,`previousElementsibling`都是上一个兄弟,这两个和上面的相同.

```
//兼容写法
var one = document.getElementById('one');
var next = one.nextElementSibling || one.nextSibling;
```

#### 第一个节点和最后一个节点

`firstChild`,`lastChild`:会识别文本节点

`firstElementChild`,`lastElementChild`:只识别标签

```
兼容写法
 var par = document.getElementById("par");
 var fist = par.firstElementChild || par.firstChild;
 var last = par.lastElementChild || par.lastChild;
```

#### 父元素的所有子节点

`childNodes`: 包含文本节点，比如空格和换行      
`children`: 仅仅包含标签,不用考虑兼容性，都适用。

```
var par = document.getElementById("par");
var childs = par.children;
var childs1 = par.childNodes;
//其中childs.length 要大于等于childs.length
```

### DOM节点操作

#### 创建节点

```
var test = document.createElement("div");
  var test1 = document.createElement("span");
  创建div节点和span节点
```

#### 添加节点

```
a.appendChild(node), 将node节点添加到 a的末尾，并且node节点是a节点子节点
var a = document.getElementById("par");
var node = document.createElement("div");
a.appendChild(node); //a节点末尾添加

a.insertBefore(newnode, refnode), 将newnode节点插入到a节点的refnode之前。
var a = document.getElementById("par");
var node = document.createElement("div");
a.insertBefore(node, a.fistElementChild);// 插入节点
a.insertBefore(node, null); //类似于appenchild, 插入到末尾
```
