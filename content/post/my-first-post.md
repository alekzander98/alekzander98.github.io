﻿---
title: 关于记事本中文乱码调试

date: 2018-10-03 00:59:50
tags: [Hexo, School]
---
发现hexo主页显示乱码问题 ,尝试解决后,进行实验!
<!--more-->
原来是配置文件的编码方式不对!
## 解决办法:

将hexo目录下及them目录下的_config.yml文件用记事本打开--另存为--将右下方编码改为UTF-8--保存.运行本机服务器查看--哇--问题解决了!


## 有料:


编码是个什么鬼，为什么编码改了之后就没有乱码了？这里科普下各种编码方式。


### 字符编码详述

计算机只能识别0和1，因此要想在计算机上显示各种字符就要把字符用二进制数来表示，这就是字符编码。（这里就不区分字符集和字符编码了）


### ASCII

这是大家最熟悉也是最早的字符集，用一个字节来表示英文字母、数字、控制符等128个字符，因此只能编码英文。


### GBK/GB18030

GBK是汉字国标扩展码，可以和Unicode中的汉字编码一一对应。GB18030是汉字国家标准代码。


### ANSI

ANSI是各种汉字延伸编码方式的总称。因为汉字也分为简体、繁体，而且日文中也有汉字。例如简体中文系统中的GB2312编码，日文系统中的JIS编码。不同ANSI编码不兼容。


### Unicode

各个国家的文字不同，需要编码的字符不同，因此要想让计算机能解析世界上所有的语言，就需要一个通用的编码方式。Unicode编码用多个字节来编码字符，是集成了世界上所有字符的一个符号集合，每一个二进制数据对应唯一的一个字符。（C语言用\0作为字符串结尾，而Unicode中有很多字符都有一个字节为0，因此C语言的字符串函数无法正常处理Unicode）

### UTF-8编码 

Unicode编码虽然解决了字符大一统的问题，但英美人民却不乐意了，英文只用一个字节来编码就可以了，为什么也要用多个字节，这不是白白浪费存储空间吗？因此就有了UTF-8这种Unicode的实现方式，它是一种智能的编码方式，用1~4个字节来表示一个字符，根据字符的不同可以变化字节长度。比如，当存储英文时就用1个字节，当存储中文时就用两个字节。
这里只是简单理解，可能稍有不妥，不去深究。
