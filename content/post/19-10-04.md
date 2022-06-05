---
title: Python之格式化
date: 2019-10-04 18:06:17
tags: [Python]
---
一个字符串可以使用某些特定的格式（Specification）通过调用一些方法,替换其中的参数,使内容更加简洁,明了.
有时候我们会想要从其他信息中构建字符串。这正是 format() 方法大有用武之地的地方。
内容参考--《A Byte of Python》
<!--more-->
你可以通过 http://python.swaroopch.com/ 在线阅读本书英文原版。



## For 1
将以下内容保存为文件 str_format.py ：
```bash
	age = 20
	name = 'Swaroop'
	print('{0} was {1} years old when he wrote this book'.format(name, age))
	print('Why is {0} playing with that python?'.format(name))
```
输出:
```bash
	Swaroop was 20 years old when he wrote this book
	Why is Swaroop playing with that python?
```

### 原理
调用format 方法,使与其相对应的参数实现替换.

在这里要注意我们第一次应用这一方法的地方，此处 {0} 对应的是变量 name，它是该格式化方法中的第一个参数。与之类似，第二个格式 {1} 对应的是变量 age，它是格式化方法中的第二个参数。请注意，Python 从 0 开始计数，这意味着索引中的第一位是 0，第二位是 1，以此类推。

当然,我们通常可以通过联立字符串达到相同效果,如:
```bash
	name + 'is' + str(age) + 'years old'
```
但这样的方式有点丑陋,容易出错.其次，转换至字符串的工作将由 format 方法自动完成，而不是如这般需要明确转换至字符串。再次，当使用 format 方法时，我们可以直接改动文字而不必与变量打交道，反之亦然。

同时,括号内的数字是一个可选选项,可以省略,即:
```bash
	age = 20
	name = 'Swaroop'
	print('{} was {} years old when he wrote this book'.format(name, age))
	print('Why is {} playing with that python?'.format(name))
```
大大减少了出错的概率,还能达到相同的效果.
### And Then
format 方法可以和参数一起使用,对参数实现更加具体的定义,例如:
```bash
# 对于浮点数 '0.333' 保留小数点(.)后三位
print('{0:.3f}'.format(1.0/3))
# 使用下划线填充文本，并保持文字处于中间位置
# 使用 (^) 定义 '___hello___'字符串长度为 11
print('{0:_^11}'.format('hello'))
# 基于关键词输出 'Swaroop wrote A Byte of Python'  
print('{name} wrote {book}'.format(name='Swaroop', book='A Byte of Python'))
```
输出:
```bash
0.333
___hello___
Swaroop wrote A Byte of Python
```
## For 2
在Python中 print 总是会以一个不可见的“新一行”字符（\n）结尾，因此重复调用 print将会在相互独立的一行中分别打印。为防止打印过程中出现这一换行符，你可以通过 end 指定其应以空白结尾,例如：
```bash
# 1.当 end 表示以空白结尾时
print('a', end='')
print('b', end='')
print('c')
# 2.当 end 指定以空格结尾时
print('a', end=' ')
print('b', end=' ')
print('c')
```
输出内容如下:
```bash
abc
a b c 
```
## 实践
经典问题一:
在Python中打印九九乘法表
```bash
row=1
while row<=9:
    col=1
    while col<=row:
        print("{}*{}={}" .format(col,row,(row*col)),end="\t")
        # \t --转义字符:制表
        col+=1
    print()
    row+=1
```
效果如下:
```bash
1*1=1	
1*2=2	2*2=4	
1*3=3	2*3=6	3*3=9	
1*4=4	2*4=8	3*4=12	4*4=16	
1*5=5	2*5=10	3*5=15	4*5=20	5*5=25	
1*6=6	2*6=12	3*6=18	4*6=24	5*6=30	6*6=36	
1*7=7	2*7=14	3*7=21	4*7=28	5*7=35	6*7=42	7*7=49	
1*8=8	2*8=16	3*8=24	4*8=32	5*8=40	6*8=48	7*8=56	8*8=64	
1*9=9	2*9=18	3*9=27	4*9=36	5*9=45	6*9=54	7*9=63	8*9=72	9*9=81	
```
