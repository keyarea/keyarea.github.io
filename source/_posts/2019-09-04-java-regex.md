---
title: Java 正则表达式
date: 2019-09-04 16:53:33
tags:
- Java
- regex
category:
- Java
---


> 通过之前一系列的分析，String是一个非常万能的类，因为String不仅仅可以支持有各种字符串的处理操作，也支持有向各个数据类型的转换，所以在项目的开发之中，只要是用户输入的信息基本上都用String表示。
> 于是在向其他数据类型转换时候为了保证转换的正确性，往往需要对其进行一些复杂的验证处理，那么这种情况下如果只是单纯的依靠String类中的方法是非常麻烦的。

## 认识正则表达式

> 现在假设有一个字符串要求你判断字符串是否由数字所组成，如果由数字所组成则将其变为数字进行乘法计算。

```java
package 正则表达式;

public class StringToIntTest {
    public static void main(String[] args) {
        String str = "123213";
        if(isNumber(str)) {
            int num = Integer.parseInt(str);
            System.out.println(num * 2);
        }
    }

    public static boolean isNumber(String str) {
        char[] list = str.toCharArray();
        for (int i = 0 ; i < list.length ; i++) {
            if(list[i] > '9' || list[i] < '0') {
                return false;
            }
        }
        return true;
    }
}
```

> 实际上这种验证的功能是非常简单的，但是如此简单的功能却需要开发者编写大量的程序逻辑代码，那么如果是更加复杂的验证呢？那么在这样的情况下，对于验证最好的做法就是利用正则表达式来完成。

范例：使用正则表达式实现相同的功能

```java
package 正则表达式;

public class RegTest {
    public static void main(String[] args) {
//        String str = "sdf";
        String str = "123";
        if(str.matches("\\d+")){
            int num = Integer.parseInt(str);
            System.out.println(num * 2);
        }
    }
}
```

> 正则表达式最早是从Perl语言发展而来的，而后在JDK1.4以前如果需要使用到正则表达式的相关定义则需要单独引入其他的*.jar文件，但是从JDK1.4之后已经默认被支持。
>
> 并且提供有java.util.regex开发包，同时针对String类也进行了一些修改，使其有方法可以直接支持正则处理。
>
> 使用正则最大的特点在于方便进行验证处理，以及方便进行复杂字符串的修改处理。

## 正则标记(记住)

> 如果要想进行正则的处理操作，那么首先需要对常用的正则标记有所掌握，从JDK1.4开始，提供有java.util.regex开发包，这个包里面提供有一个Pattern程序类，在这个程序类里面定义有所有支持的正则标记。

> 单个字符匹配

```text
任意字符：表示由任意字符组成;
\\: 表示匹配\;
\n: 标识匹配换行;
\t: 匹配制表符;
```

> 单个字符集匹配(可以从中任选一个字符)

```text
[abc] : 表示可能是字符a,b,c中的任意一个;
[^abc] : 表示不是由字母a,b,c中的任意一个;
[a-zA-Z] : 表示任意一个字母所组成，不区分大小写;
[0-9] : 表示由一位数字所组成;
```

> 简化的字符集

```text
. : 表示任意的一个字符;
\d : 表示任意一个数字，等价于[0-9];
\D : 表示不是数字，等价于[^0-9];
\s : 表示匹配任意的一位空格，可能是空格、换行、制表符;  
\S : 表示匹配任意的非空格数据;
\w : 表示匹配字母、数字、下划线，等价于[a-zA-Z_0-9];
\W : 表示匹配非字母、数字、下划线，等价于[^a-zA-Z_0-9];
```

> 边界匹配

```text
^ ：匹配边界开始;
$ : 匹配边界结束;
```

> 数量表达,默认情况下只有添加上了数量单位才可以匹配多位字符;

```text
表达式? : 表示该表达式可以出现0次或一次;
表达式* : 表示该表达式可以出现0次1次或多次;
表达式+ : 表示该表达式可以出现1次或多次;
表达式{n} : 表示该表达式出现n次;
表达式{n,} : 表示该表达式出现n次或n次以上;
表达式{m,m} : 表示该表达式出现n-m次;
```

> 逻辑表达式: 可以连接多个正则

```text
表达式X表达式Y ：X表达式之后紧跟上Y表达式;
表达式X|表达式Y ：有一个表达式满足即可;
(表达式): 为表达式设置一个整体描述，可以为整体描述设置数量单位;
```

## String类对正则的支持

> 在进行正则表达式大部分处理的情况下都会基于String类来完成，并且在String类里面提供有如下与正则有关的操作方法:

---

> `public boolean matches(String regex)`

将指定字符串进行正则判断

---

> `public String replaceAll(String regex, String replacement)`

替换全部

---

> `public String replaceFirst(String regex, String replacement)`

替换首个

---

> `public String[] split(String regex)`

正则拆分

---

> `public String[] split(String regex, int limit)`

正则拆分

---

范例：实现字符串的替换（删除掉非字母与数字）

```java
public class RegexTest1 {
    public static void main(String[] args) {
        String str = "djfosdjfoj3j4o3#@#@OJOjojoj@J#$JOjoj";
        String regex = "[^a-zA-Z0-9]+";
        System.out.println(str.replaceAll(regex, ""));
    }
}
```

--- 

范例：实现字符串的拆分

```java
import java.util.Arrays;

public class RegexTest2 {
    public static void main(String[] args) {
        String str = "fi2121d2jf3oij1fj";
        String regex = "\\d+";
        String[] result = str.split(regex);
        System.out.println(Arrays.toString(result));
    }
}
```

> 在正则的处理的时候对于拆分与替换的操作相对容易一些，但是比较麻烦的是数据验证部分。

范例：判断一个字符串是否为小数，如果是小数则将其变为double类型

```java
public class RegexTest3 {
    public static void main(String[] args) {
        String str = "100.1";
        String regex = "\\d+(\\.\\d+)?";
        System.out.println(str.matches(regex));
    }
}
```

---

范例：判断一个字符串是否是由日期所组成，如果是由日期所组成则将其转为Date类型

```java
import java.text.SimpleDateFormat;

public class RegexTest4 {
    public static void main(String[] args) throws Exception{
        String str = "1994-09-12";
        String regex = "\\d{4}-\\d{2}-\\d{2}";
        if(str.matches(regex)) {
            System.out.println((new SimpleDateFormat("yyyy-MM-dd")).parse(str));
        }
    }
}
```

> 需要注意的是，正则表达式无法对里面的内容进行判断，只能对格式进行判断处理

范例：验证email地址:
- email的用户名可以由字母、数字、下划线所组成（不应该使用`_`开头）;
- email的域名可以由字母、数字、_、-所组成;
- 域名的后缀必须是：.net,.com,.cn,.com.cn,.org,.gov

```java
public class RegexTest5 {
    public static void main(String[] args) {
        String str = "mail@qipo.net";
        String regex = "[a-zA-Z0-9]\\w+@\\w+.(net|com|cn|org|gov|com.cn)";
        System.out.println(str.matches(regex));
    }
}
```

## java.util.regex开发包

> 虽然在大部分的情况下都可以利用String类实现正则的操作，但是也有一些情况下需要使用到java.util.regex开发包中提供的正则处理类。
>
> 在这个包中一共定义有两个类：Pattern（正则表达式的编译类），Matcher（匹配类）。

### Pattern类

> Pattern类提供有正则表达式的编译处理支持：`public static Pattern compile(String regex)`
>
> 同时也提供有字符串的拆分操作支持: `public String[] split(CharSequence input)`


范例： 字符串拆分（字符串按照非字母进行拆分操作）

```java
import java.util.Arrays;
import java.util.regex.Pattern;

public class RegexTest6 {
    public static void main(String[] args) {
        String str = "fjsdojo3ojf9gjdfoj4ojfjos3423jofjdosjf";
        String regex = "[^a-zA-Z]+";
        Pattern pat = Pattern.compile(regex);
        String[] result = pat.split(str);
        System.out.println(Arrays.toString(result));
    }
}
```

### Matcher类

> 实现了正则匹配的处理类，这个类的对象实例化依靠Pattern类完成：
> - Pattern类提供的方法： `public Mathcher matcher(CharSequence input)`;
> 当获取了Matcher类对象之后就可以利用该类中的方法进行如下操作：
> - 正则匹配：`public boolean matches()`;
> - 字符串替换：`pulice String replaceAll(String replacement)`;

---

范例：字符串匹配

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexTest7 {
    public static void main(String[] args) {
        String str = "101";
        String regex = "\\d+";
        Pattern pat = Pattern.compile(regex);
        Matcher mat = pat.matcher(str);
        System.out.println(mat.matches());
    }
}
```

> 如果纯粹的是以拆分、替换、匹配三种操作为例根本用不到java.util.regex开发包，只依靠String类就都可以实现了。

范例： 找出字符串中的所有数字

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexTest8 {
    public static void main(String[] args) {
        String str = "fjodsijfoi3jjojoi34234jfo3423";
        String regex = "\\D+";
        Pattern pat = Pattern.compile(regex);
        Matcher mat = pat.matcher(str);
        System.out.println(mat.replaceAll(""));
    }
}
```

---

> 但是Matcher里面提供有一种分组的功能，而这种分组的功能是String所不具备的。   

范例：要求取出${}里面的所有内容

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexTest9 {
    public static void main(String[] args) {
        String str = "hello${var},my name is ${name}";
        String regEx = "\\$\\{\\w+\\}";
        Pattern pat = Pattern.compile(regEx);
        Matcher mat = pat.matcher(str);
        while (mat.find()) { // 是否有匹配成功的内容
            System.out.println(mat.group(0).replaceAll("\\$|\\{|\\}", ""));
        }
    }
}
```

> java.util.regex开发包，如果不是进行一些更为复杂的正则处理是很难使用到的，而String类所提供的功能只适合于正则的基本操作。

