---
title: Python之函数
date: 2019-11-10 10:29:37
tags: [Python]
---
学习一门语言,必不可少的你要了解它对函数的使用.在编程中允许你主动为某个代码块赋予名字,使这一代码块独立,在这之后你可以在任何位置通过名字运行代码块,并可重复任何次数,这就是所谓的调用（Calling）函数（Functions）。
内容参考--《A Byte of Python》
<!--more-->
你可以通过 http://python.swaroopch.com/ 在线阅读本书英文原版。
中文译版可通过 https://bop.molun.net 在线阅读。


## 函数定义
Python中函数可以通过关键字-- def --来定义.在 def 后是你为函数起的名字(不可重复,必须是唯一的),再跟一对圆括号，再以冒号结尾，结束这一行。随后而来的内容是函数的一部分。
格式:
```bash
def 函数名():
	内容
	内容
```
通过--函数名()--实现对函数的调用,可进行多次调用,方法一样.
```bash
函数名()  #第一次调用
函数名()  #再次调用
```
### 注意
函数定义时,函数的每行内容都要缩进,当有一行未进行缩进,代表函数定义结束.
## 函数参数
在函数定义时,圆括号内可以包括一些变量的名称用来指定函数中会用到的参数,此时在进行函数调用时也需要在括号内添加对应类型的参数.
案例:
```bash
def print_max(a, b): # 寻找最大值函数
    if a > b: # if...else 语句
        print(a, 'is maximum')
    elif a == b:
        print(a, 'is equal to', b)
    else:
        print(b, 'is maximum')

#对函数内参数值的传递有两种方式:

# 1.直接传递字面值
print_max(3, 4) # 函数调用

# 2.以参数的形式传递变量
x = 5
y = 7

print_max(x, y) # 函数调用
```
输出:
```bash
4 is maximum
7 is maximum
Process finished with exit code 0
```
## 局部变量
在函数定义中,括号内的变量不会对函数外的函数造成影响,它只作用于当前函数内,这被称为变量的作用域（Scope).
例如:
```bash
x = 50


def func(x): #定义一个带参数x的函数func
    print('x is', x)
    x = 2 #对函数内的参数x进行赋值
    print('Changed local x to', x)


func(x)
print('x is still', x) #这里输出的是函数外的x值,用来展示主代码块中的x未发生变化
```
结果:
```bash
x is 50
Changed local x to 2
x is still 50
Process finished with exit code 0
```
### Then
当你需要在函数中对主代码块中的参数进行修改,就需要用到接下来要了解的内容:--global语句.你应该告诉程序这里修改的参数是全局的,不只用在函数块中.

## global语句
global 语句用以声明 x 是一个全局变量——因此，当我们在函数中为 x 进行赋值时，这一改动将影响到我们在主代码块中使用的 x 的值。
例如:
```bash
x = 50


def func(): #注意! 注意! 注意!
    global x

    print('x is', x)
    x = 2
    print('Changed global x to', x)


func()
print('Value of x is', x)
```
结果为:
```bash
x is 50
Changed global x to 2
Value of x is 2

Process finished with exit code 0
```

需要注意的是:若使用global语句,定义函数时,不允许在括号内添加相同名称的变量,使用之前定义的变量.否则,会提示以下错误:
```bash
SyntaxError: name 'x' is parameter and global
#语法错误：名称'x'是参数和全局
```
同时,可以在同一句 global 语句中指定不止一个的全局变量，例如 global x, y, z。

## 关键字参数
这部分内容,我认为原文说的不够明确,以下内容只代表我的理解:
在定义新函数时,可以对括号内要添加的参数使用关键字命名,这样在下面调用函数时,可以通过关键字准确的对某个参数进行赋值和修改,这需要其他的参数也要具有默认值.如若不使用关键字,会对括号内的参数按照顺序依次赋值/修改.

## 可变参数
内容包含了--元组与字典--的知识,学习后再进行补充(emmm)
--等待补充--

## return 语句
return 语句用于从函数中返回，也就是中断函数。我们也可以选择在中断函数时从函数中返回一个值。
例如:
```bash
def maximum(x, y):
    if x > y:
        return x
    elif x == y:
        return 'The numbers are equal'
    else:
        return y

print(maximum(2, 3))
```
输出：
```bash
3
Process finished with exit code 0
```
return语句,学习过C语言或其他语言的都不会陌生,在程序结束时返回0,一般会默认添加;表示中断作用时,类似的还有 break语句;continue语句;

## DocStrings
简单地说,在函数块中可以使用三对单引号书写,保存一段文档,在需要显示的地方添加以下语句调用:
```bash
#1.
print(函数名.__doc__) #注意其中的双下划线
#2. help()函数也能显示函数内的文档信息
help(print_max)
```
这时我们已经了解到了每天日常使用都会使用到的 Python 函数.