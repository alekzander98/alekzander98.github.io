---
title: 原生JavaScript基础
date: 2021-03-15 07:50:30
categories:
tags: [JavaScript, Study]
---

1. ##### JavaScript的引入方式

2. ##### 常用方法

3. ##### 基本数据类型

4. ##### 数据类型转换

<!--more-->

### 脚本后的标签调用不到

### 标签上的script脚本

- #### HTML标签中只有事件可以添加JS

- #### 超链接a的href中可以添加`javascript:console.log(‘abc’);`

  1. ##### `<a href="javascript:console.log('cde')">超链接</a>`

  2. ##### `<a href="javascript:void(0)">超链接</a>`

  3. ##### `<a href="#">超链接</a>`

     > ##### void(0)不会产生浏览记录。#可以刷新页面

### 外部引入1

#### 写下`head`标签内部

`<script src="./a.js"></script>`

##### **拥塞  *同步***

```html
<script src="./a.js"></script>
<script src="./b.js"></script>
```

###### 当有多个引入标签时，上一个标签加载解析执行完才会引入下一个标签内容

**拥塞  *异步***

###### 当script标签中添加`async`可以同时加载

```html
<script src="./a.js" async></script>
<script src="./b.js" async defer></script>
```

#### `defer` 在最后执行的代码

> *a.js中想要调用b.js的内容*

### DOM树+CSS树===>>渲染树

### 外部导入2

- ```html
  <script type="module">
  	import a from "./b.js";
  </script>
  ```


### 注释

- ##### 标签中，script中都可，行注释：`Ctrl+/`,块注释：`alt+shift+A`。

- ##### 混淆代码：   *块注释*

- ##### `TODO`高亮注释标识未来需要继续编写的代码。

### 控制台：

```javascript
console.log("aaa");
console.trace("abc");
console.debug("abc");
console.error("这里有错误");
```

### 内存泄漏

### 常用方法：

1. ##### `alter("弹出窗");`

2. ##### `corfirm("提示框/确定取消框");`

3. ##### `prompt("今年你多大了",18);`输入弹出框

4. ```js
   var div1=document.getElementById("div1");
   div1.innerHTML="abc";
   div1.style.color="red";
   div.style.backgroundColor="blue";
   div1.onclick=function(){
       div1.innerHTML="我点击了";
   }
   ```

   > 设置行内样式

### 变量：

- #### 每个变量都应该先声明

- #### 变量的名称不能使用数字开头，必须使用（$/_）或者大小写的英文字母。

- #### 关键词和保留字不能作为变量名。

- #### 变量命名：

  - ###### 普通变量名使用小写字母开始，*驼峰式命名法*，变量名要有意义。

    ```javascript
    var getLISTCount
    ```

  - ###### 临时变量、参数、局部变量使用_起头，驼峰式命名法

    ```javascript
    var _num=5;
    ```

  - ###### ES5中变量实际上是赋值给了window对象的一个属性==>不能使用window中的属性或者方法名作为变量名。

    > ###### 控制台蓝色数值 黑色字符串

  - ###### 常量:ES6中才有常量，常量只有在第一次定义并且赋值，以后不能修改其值。

    ###### 常量全部大写，并且使用_分割单词。

    ```javascript
    const EVENT_ID="string"
    ```

- #### 数据类型：

  ###### 弱类型：定义的变量类型可变

  ```javascript
  var a=4;
  a=5;
  a="abc";
  ```

  ###### 弱类型不需要定义变量的类型，可以随意更改这个变量值得数据类型。

  1. ##### 字符型：

     - ```javascript
       var a='abc';
       var a1="abc";
       var a2=`abc`;
       ```

  2. ##### 数值型：

     - ```javascript
       var b=1;
       var b1=1.2;
       var b2=-2;
       var b3=0x01;//16进制数值型
       var b4=0047;//8进制数值型
       var b5=1.23e+5;//科学计数法
       var b6=2.3e-6;//科学计数法
       ```

  3. ##### 布尔值（true/false）

  4. ##### 未定义

     - ```javascript
       var d=undefined;
       var d1;
       ```

  > ###### 一个定义为undefined，一个未定义默认为undefined。

  5. ##### 空（null）：

     ###### 针对对象设置为null，用于清除对象的引用关系

  6. ##### 对象：

     - ###### `var o={a:1,b:2};`

- #### 数据类型转换：

  1. ##### 数值型转换为字符型

     ```javascript
     var a=5;
     ```

     1. ##### 强制转换，通过String构造函数方法转换

        - `String(a);`

     2. ##### 字符转换

        - ```javascript
          a=a.toString();
          ```

        > ###### 进制转换：参数中是数字，表示将变量转换为该数值的进制字符串
        >
        > ```javascript
        > a=a.toString(2);//2进制
        > a=a.toString(16);//16进制
        > ```

2. ##### 保留小数点后几位：

   - ```javascript
     a=a.toFixed();
     //带参数表示保留小数点后几位
      //四舍五入效果
     ```

  3. - ##### 大于0时，保留几位数值并且按照科学计数法来写

     - ##### 小于1时，保留几位有效数字

       - ```javascript
         a=a.toPrecision(2);
          console.log(a);
         ```

  4. ##### 保留小数点后几位的科学计数法

     - ```javascript
       a=a.toExponential(3);
       console.log(a);
       ```