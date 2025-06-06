---
title: Symbol/Set/Map与模块化开发
date: 2021-04-02 19:13:35
categories:
tags: [JavaScript,ES6]
---

- 字符串方法
- Symbol
- Set/Map
- 模块化开发

<!--more-->

### 字符串方法和Symbol:

- `startsWith("",)`从某个下标开始起始字符是不是某个字符

- `endsWith("",)`判断某个下标前一个是不是这个字符

- `"".reprat(3)`重复几次

- 判断字符串的长度不足某个值时，在前面补某个字符。

- `"".padStart(3,"");`

- `"".padEnd(3,"");`

- `${变量名}`

- ```javascript
  var i=1;
  var str=`aa${i}aa`;
  var sre="aa"+i+"aa";
  ```

- ##### Symbol:值唯一，不可能重复。

- 用在对象中；对象的Key，只能是字符串或者Symbol类型，如果不是就隐式转换为字符串

- ###### Symbol 做为Key时，无法通过for in 遍历；

### Set和Map：：

- **数组**：有序的列表，从0开始，可以根据指定的下标找到元素，以及其上下关系的元素，是一种紧密结构，长度会随着添加或者删除而改变；

  - 作用：主要存储相关类似的数据，便于遍历全部处理。
  - 缺点：在插入和删除时消耗性能；排序也非常消耗性能。元素没有宿主，元素本身是无关联性的，数组的元素可以重复。查找速度很慢。
  - 迭代器

- **对象**：无序列表，键值对形式存储。可以通过键找到值，查找速度非常快。

  - 用来存储关联性数据；因为无序，插入和删除速度非常快。
  - 缺点：key只能是字符串或者Symbol，无法使用对象或者其他内容作为键；对象是没有长度，无法确定对象是否遍历到最后一个，只能通过键名找到值，无法直接使用值找到键名，需要遍历查找才可以。
  - 键找值：速度快；值找键：速度慢。
  - 无迭代器

- ##### Set:

- 无序列表，集或者集合，为了解决添加删除查找速度快，将数据以无序的方式存储，自带值去重，无重复列表，有一个维护性的元素数量

- ```javascript
  var s=new Set();//创建
  //添加
  s.add(1);
  s.add(2);
  s.add(3);
  s.delete(2);//删除
  s.clear();//清空
  console.log(s.size)//长度
  console.log(s.has(2));//判断有没有某个元素：返回值--false、true
  ```

- Set去重：

- ```javascript
  arr=Array.form(new Set(arr));
  console.log(arr);
  ```

- 具有迭代器的类型使用for...of遍历

- ```javascript
  for(var value of s){
      console,log(value);
  }
  //集合是没有下标的
  ```

- `forEach`：遍历

- `WeakSet`：弱引用

- 存在WeakSet中的引用对象被设为null，垃圾回收车会自动从列表中清除。不能被遍历

- ##### Map:类型

- 作用：针对Object做了优化，模仿HashMap，

- key可以是任何类型，有size--长度。

- ```javascript
  var map=new Map();
  map.set("name","值");//添加数据
  var arr=[1,2,3];
  map.set(arr,10);
  ```

- 判断有没有某个键

- ```javascript
  map.get("name");//有返回value，没有返回undefined
  map.has(arr);//返回true和false。
  ```

- 删除：按照键名

- ```javascript
  map.delete(arr);
  ```

- 清空

- ```javascript
  map.clear();
  ```

- 遍历键、值

- ```javascript
  map.keys();
  map.value();
  for(var key of map.keys()){
  	console.log(key);
  }
  for(var [key,value] of map){
  	console.log(key,value);
  }
  //迭代器遍历
  for(var [key,value] of map.enties()){
  	console.log(key,value);
  }
  //forEach 完成遍历
  map.forEach((value,key)=>{
      console.log(key,value);
  })
  ```

- WeakMap：弱引用，键必须是对象

- 不能遍历

### 生成器函数：

### 类：

- 类--基类--超类--父类--子类--实例化对象--继承
- 类别就是实例对象的抽象体现
- 实例对象就是类别的具象表现
- 子类的父类叫做**超类**
- static定义的属性是类自身的属性和方法，实例化的对象不会拥有
- 继承：extends
- 继承的类需要执行其超类的构造函数---在子类的构造函数中添加 `super();`
- Object 是所有类别的基类

### 模块化开发：

- ```html
  <script type="module">
  	import Ball from "./js/Ball.js";
      import {A,fn1 as fn2,obj} from "./js/Other.js";// fn1 as fn2 起别名
  </script>
  ```

- 构造函数名统一叫 `constructor`

- new 类  就是执行该类的constructor函数

- 静态属性、方法中禁止使用this，使用类名调用。

- ```javascript
  var obj={a:1,b:2};
  var o={c:1,...obj};//实现复制obj，浅复制
  obj,a=10;
  console.log(o);
  ```

##### 多选框和单选框