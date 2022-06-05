---
title: DOM属性相关
date: 2021-03-27 16:34:56
categories:
tags: [JavaScript]
---

1. DOM的对象属性
2. 标签属性
3. CSS样式获取、写入
4. 纯函数概念

<!--more-->

### DOM属性

- 标签属性：标签上表明的属性

- 标签属性分为**默认标签属性**和**自定义标签属性**

- 自定义属性的命名和值必须使用小写和横线连接

- DOM的对象属性也也分为默认属性和自定义属性

- DOM的对象属性和标签属性 不一定相同 也不一定可以互相获取

- DOM中的自定义属性和标签自定义属性是完全不能互相调用

- 有些标签默认属性可以通过DOM属性调用

  - > title	id	src	href	alt	placeholder	type	value等

- ##### class标签属性DOM属性调用时`className`

- ##### 先有标签，才可以渲染对象调用，也可以通过DOM对象设置标签的属性来达到重新渲染

- ##### 通过标签属性的修改添加才会引起重绘，增加对象属性不会引起重绘

  - > checked是标签属性checked的对象映射属性

- DOM对象.setAttribute("标签属性名","属性值")//设置标签属性

- DOM对象.removeAttribute("标签属性名","属性值")//删除标签属性

- DOM对象.getAttribute("标签属性名","属性值")//根据标签属性名获取值

  - 标签属性可以直接看到，标签对象属性无法直接看到

- 通过`div.style`添加标签的行内样式

- style属性只能给DOM对象设置行内样式

- ```javascript
  var div=document.createElement("div");
  div.style.width="50px";
  div.style.height="50px";
  div.style.backgroundColor="red";
  document.body.appendChild(div);
  ```

- **对象属性样式写入**

- 设置样式时，只能设置非伪类、伪元素的DOM样式

- 设置样式时，如果样式属性名多个词汇组成 background-color，设置时去掉-并且把后面的字母变成大写=backgroundColor

- `div.style="width:50px;height:50px;background-color:red";`

- **直接行内样式写入**

- `div.style="width:50px;height:50px;background-color:red";`

- **获取样式**：

- `console.log(div1.getComputedStyle(div1).width)`

- 可以获取行内样式也可以获取CSS样式

- 有兼容问题，IE8及以下没有

- IE8及以下使用：

- ```javascript
  console.log(div1.currentStyle.left);
  console.log(div1.currentStyle.width);
  ```

- **CSS样式**

- 获取：

- `var styleSheet=document.styleSheets[document.styleSheets.length-1];`

- 添加样式标签：

- ```javascript
  //IE都支持
  styleSheet.addRule(".div1","width:50px;height:50px;background-color:red",0);
  styleSheet.addRule(".div1:hover","background-color:blue",1);
  //IE9以上支持
  styleSheet.insertRule(".div1 {width:50px;height:50px;background-color:red}",0)
  ```

### Object.assign()用于对象的合并


### 纯函数：

没有调用全局的变量或者其他函数，独立自主的某一功能的函数，这种函数一般回通过参数传入需要处理的内容，完成后返回===抽象：不是解决某一个或者某一件具体的事情。