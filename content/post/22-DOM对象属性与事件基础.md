---
title: DOM对象属性与事件基础
date: 2021-03-29 22:44:39
categories:
tags: [JavaScript,事件,设计模式]
---

1. DOM对象常用属性
2. 观察者模式、中介者模式
3. 事件委托

<!--more-->

### DOM对象常用属性：

- 获取和设置都会引起浏览器重绘

- 必须已经放在页面中渲染的DOM对象元素

  - 放入body页面完成渲染之前无法获取

- 宽高  位置

- 鼠标位置通过X、Y设置

- DOM对象位置通过left、top设置

- document对象

- body对象

- clientWidth/Height  客户宽高 边框以内的所有宽高之和-滚动条宽高===包含padding，不包含边框；

- offseWidth/Height  偏移宽高  DOM元素在页面中占位的宽高===包含边框

- scrollWidth/Height  滚动区域宽高

- body标签document.body的滚动区域宽度会根据设置内容的宽度改变

- html标签document.documentElement的滚动区域宽度不变

- clientLeft/Top 客户位置  边线宽高

  - border

- offsetLeft/Top 偏移位置  位置的左上点  元素左上角的相对位置

  - 设置定位时，相对父元素

- scrollLeft/Top 滚动条位置

  - 可读可写
  - body标签和html标签的滚动条在初始化第一次渲染时，页面中的滚动条不可以设置，当第一次渲染完毕，交互时有效（早期不支持）；body标签（最新版不支持）。

- ##### 案例：回到顶部的滚动条过渡效果

### 事件：

事件驱动型语言

注入式语言

耦合/解耦

- Event

  - 侦听接收
  - 抛发

- 系统事件

- ##### 自定义事件

  - 如果需要接收和抛发事件，必须要有对应的对象，接收和抛发的对象；对象必须是同一个，而且必须是继承EventTarget类型的对象===所有的DOM对象都是可以接收和抛发事件的

  - ```javascript
    document.addEventListener(事件类型,事件执行函数，是否捕获阶段触发/事件配置对象);
    var evt=new Event(事件名);
    document.dispatchEvent(evt);//抛发事件
    function 函数(e){};
    ```

  - 接收和抛发对象必须一致，侦听事件类型必须和抛发事件类型一致，侦听在前，抛发在后。

  - 侦听事件的函数中，有且仅有一个参数，就是抛发的事件对象。

- ##### 解耦案例：

- ##### 观察者模式、中介者模式

- 创建事件目标对象：

- ```javascript
  var t = new EventTarget();
  t.addEventListener(事件名,函数);
  ```

- 事件是针对事件目标对象的，来抛发内容；但并不是直接抛发过去，尤其是针对DOM元素，根据树形结构来传递事件。

- ##### 事件过程：捕获阶段（从外向内）、目标阶段、冒泡阶段（从内向外）

- ```javascript
  document.addEventListener(事件类型,事件执行函数，是否捕获阶段触发/事件配置对象);
  ```

- 事件类型(必须是字符串)：

- 系统事件类型，分为多个类型：Event、MouseEvent

- 事件执行函数：有且仅有一个参数，不能通过事件直接传入参数内容，因为**事件执行函数是一个事件处理后回调的函数，所以只能传入函数名**；**如果传参就需要执行才可以**，但是回调函数必须是函数名，因此不能在这里传参。

- 事件执行函数是一个回调执行的，在事件处理中并没有处理这个回调执行的结果===事件执行函数不能使return返回数据

- 事件执行函数里面的this发生了改变。

- 事件是否在捕获阶段被触发，默认是false===冒泡阶段触发。===改变事件触发顺序

- `e.stopPropagation();`事件对象中的方法===阻止传递。IE8及以下：`e.cancelBubble=ture;`

- ```javascript
  var evt=new MouseEvent("click",{bubbles:true,clientX:100,clientY:100});
  div3.dispatchEvent(evt);
  // 这样{bubbles:true}就会让抛发的事件冒泡
  ```

- 事件配置对象：`{once:true}`//事件只触发一次

- 侦听的事件目标对象和被点击到的目标对象并不一定相同。

- ##### 在事件触发函数中

  - `e.target`和 `e.srcElement`事件触发的目标对象
  - `e.currentTarget`事件侦听的对象

- ##### 事件委托：将子元素或者后代元素的事件委托给父元素，减少事件侦听的增加，防止内存泄漏。