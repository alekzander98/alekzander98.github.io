---
title: flex布局
date: 2021-04-30 08:28:08
categories:
tags: [CSS3]
---

## Flex布局

```txt
Flex容器：采用 Flex 布局的元素的父元素；
Flex项目：采用 Flex 布局的元素的父元素的子元素；
容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
```

#### flex容器属性

​	1、display:flex、inline-flex

```txt
注意，设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效。
```

​	2、flex-direction属性 决定主轴的方向（即项目的排列方向）

```
flex-direction: row | row-reverse | column | column-reverse;
```

​	3、flex-wrap属性，定义子元素是否换行显示

```
flex-wrap: nowrap | wrap | wrap-reverse;
```

​	4、 flex-flow 

```
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap;
```

​	5、 justify-content属性 定义了项目在主轴（）上的对齐方式。

```
justify-content: flex-start | flex-end | center | space-between | space-around;
```

​	6、align-items属性定义项目在交叉轴上如何对齐。

```
align-items: flex-start | flex-end | center | baseline | stretch（默认值）;
```

​	7、align-content属性定义了多根轴线的对齐方式。对于单行子元素，该属性不起作用。

```
align-content: flex-start | flex-end | center | space-between | space-around | stretch;
align-content在侧轴上执行样式的时候，会把默认的间距给合并。对于单行子元素，该属性不起作用
```

#### flex项目属性

1、order

```
说明：
	number排序优先级，数字越大越往后排，默认为0，支持负数。
```

2、flex

```
说明：
	复合属性。设置或检索弹性盒模型对象的子元素如何分配空间
	详细属性值：
		缩写「flex: 1」, 则其计算值为「1 1 0%」
		缩写「flex: auto」, 则其计算值为「1 1 auto」
		flex: none」, 则其计算值为「0 0 auto」
		flex: 0 auto」或者「flex: initial」, 则其计算值为「0 1 auto」，即「flex」初始值
```

### flex布局案例

![](4.png)

[相关学习](https://www.cnblogs.com/sxz2008/p/6635196.html)