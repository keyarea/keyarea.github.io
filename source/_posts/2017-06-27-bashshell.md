---
layout: post
title:  linux shell脚本-2
date:   2017-06-27 17:14:54
categories: 
- shell
tags: 
- linux
- shell
---



# 注释

单行注释用   #    注释

多行注释:前面加  :<<!   后面加  !

# expr运算

代码:

```
a=10
b=2
sum=`expr $a + $b`   #加法    反引号用来引用语句执行结果
min=`expr $a - $b`   #减法
mul=`expr $a \* $b` #乘法
div=`expr $a / $b` #除法
yu=`expr $a % $b` #取余   
```

关于运算优先级,要加括号,不然只会先乘除后加减.括号必须如下加上  `\ ` 反义字符.还有表达式两边是    (反撇)  并不是 `'` (引号).



deshu=`expr \( $a \* $b + $c  - $d \) / $b `



# let运算

代码:

```
read a b
let " c = a * b "
echo $c
```

let ：用来执行算数运算和数值表达式测试。

let 命令的替代表示形式是: ((算术表达式)) ，这样做的好处是如果表达式中含有特殊的字符，可以告示shell 进行特殊处理.

特点:

1. 在双括号结构中，所有表达式可以像c语言一样，如：a++,b--等。

2. 在双括号结构中，所有变量可以不加入：“$”符号前缀。

3. 双括号可以进行逻辑运算，四则运算

4. 双括号结构 扩展了for，while,if条件测试运算

5. 支持多个表达式运算，各个表达式之间用“，”分开

# 逻辑判断语句

代码:

```
if test 4 -gt 3                     #也可以用   if [ 4 -gt 3 ]              -gt :是前一个数如果大于后一个数,输出true
then  echo "4 大于 3"
fi
```

必须用  if   倒过来结尾即:   fi

 1）判断表达式

- if test     (表达式为真)

- if !test     !表达式为假

- test 表达式1 –a 表达式2                  两个表达式都为真

- test 表达式1 –o 表达式2                 两个表达式有一个为真

2）判断字符串

- test –n 字符串                                   字符串的长度非零

- test –z 字符串                                    字符串的长度为零

- test 字符串1＝字符串2                    字符串相等

- test 字符串1 !＝ 字符串2               字符串不等

3）判断整数

- test 整数1 –eq 整数2                        整数相等

- test 整数1 –ge 整数2                        整数1大于等于整数2

- test 整数1 –gt 整数2                         整数1大于整数2

- test 整数1 –le 整数2                         整数1小于等于整数2

- test 整数1 –lt 整数2                          整数1小于整数2

- test 整数1 –ne 整数2                        整数1不等于整数2

4）判断文件

- test  File1 –nt  File2　　　　　　　　文件1比文件2 新

- test  File1 –ot  File2　　　　　　　　文件1比文件2 旧

- test –d File　　　　　　　　文件存在并且是目录
- test –f File 　　　　　　　 文件存在并且是正规文件

- test –e File　　　　　　　　文件存在
- test –r File　　　　　　　　文件存在并且可读
- test –w File　　　　　　　　文件存在并且可写

- test –x File　　　　　　　　文件存在并且可执行

- test  File1 –ef  File2　　　　　　　　两个文件具有同样的设备号和i结点号


- test –b File　　　　　　　　文件存在并且是块设备文件

- test –c File　　　　　　　　文件存在并且是字符设备文件

- test –g File　　　　　　　　文件存在并且是设置了组ID

- test –G File　　　　　　　　文件存在并且属于有效组I

- test –h File　　　　　　　　文件存在并且是一个符号链接（同-L）

- test –k File　　　　　　　　文件存在并且设置了sticky位

- test –b File　　　　　　　　文件存在并且是块设备文件

- test –L File　　　　　　　　文件存在并且是一个符号链接（同-h）

- test –o File　　　　　　　　文件存在并且属于有效用户ID

- test –p File　　　　　　　　文件存在并且是一个命名管道

- test –s File　　　　　　　　文件存在并且是一个套接字

- test –t FD　　　　　　　　  文件描述符是在一个终端打开的

- test –u File　　　　　　　　文件存在并且设置了它的set-user-id位



if_else语句:

代码:

```
#!/bin/bash
if [ $# -ne 1 ]
then
  exit
fi
if ! [ -e $1 ]
then
  exit
fi
if [ -d $1 ]
then
  echo "该文件是个目录"
elif [ -f $1 ]
then
  echo "该文件是个普通文件"
else
  echo "该文件是个其他类型的文件"
fi
```

# 练习

1. 在家目录下创建一个文件夹 dir,在此文件夹中创建一个文件, if_file判断这个文件是否创建成功并打印这个文件的读写执行权限.

2. 当前系统时间,判断是闰年还是平年.

# 答案

## 1.代码:


```
cd
mkdir dir
cd dir
touch if_file
if [ -e if_file ]
then
  echo "创建成功!"
fi
if [ -r if_file ]
then
  echo "可读"
else
  echo "不可读"
fi
if [ -w if_file ]
then
  echo "可写"
else
  echo "不可读"
fi
if [ -x if_file ]
then
  echo "可执行"
else
  echo "不可执行"
fi
```

## 2.代码:

```
DATE=`date`
y=${DATE:0:4}
echo $y
let "nl = y % 4 "
let "nw = y % 100"
let "nc = y % 400"
if [ $nl -eq 0 -a $nw -ne 0 ] || [ $nc -eq 0 ]
then
  echo "是闰年"
else
  echo "不是闰年"
fi
```

# 小数运算

```
value = `echo "1.1 + 2.2" | bc`
echo $value
```

# case语句

比如:输入一个成绩,根据分数给出ABCD评级

代码如下:


```
#!/bin/bash
read i
if [ $i -gt 100 -o $i  -lt  0 ]
then
  echo "您输入的不是成绩吧,你妈的,好好输"
  exit 1
else
  let " g = $i / 10 "
  case $g in
    9 | 10 )             #用 | 可以添加多个常量
    echo "A"
    ;;                    #语句结束必须;;结尾
    8 )
    echo "B"
    ;;
    7 )
    echo "C"
    ;;
    * )                  # 这就是其他
    echo "D"
  esac           # case反过来结尾,即:esac
fi
```
