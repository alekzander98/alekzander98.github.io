---
title: JS数组及常用方法
date: 2021-03-22 22:29:25
categories:
tags: [JavaScript,深复制/浅复制]
---

1. 数组相关概念
2. 数组常用方法及重构

<!--more-->

### 递归：

- ##### 函数在没有完全执行结束时，内部的所有变量都不会被销毁

- ##### 递归会建立一个函数的副本，在堆中建立一个相同的函数引用，递归时调用这个新的函数引用

- ##### *堆栈上限溢出*

- ##### 广度遍历/深度遍历：

  - ###### 对象的广度遍历使用for in;
  - ###### 对象的深度遍历使用递归；

- ##### 对象的深复制：

- ```javascript
  var obj = {
      a1: 1,
      b1: 2,
      c1: 3,
      d1: {a2: 4,b2: 5,c2: 6,
           e2: {a4: 7,b4: 8,c4: 9,
                d5: {}},
           d2: {a3: 10,b3: 11,c3: 12,
                f3: {a4: 13,b4: 14,c4: 15,
                     d5: {}},
                d3: {a4: 16,b4: 17,c4: 18,
                     d5: {}},
               },
          },
  };
  function fn1(source,target){
      if(target===undefined) target={};
      for(var prop in source){
          if(typeof source[prop]==="object" && source[prop]!==null){
              target[prop]={};
              fn1(source[prop],target[prop])
          }
          else {
              target[prop]=source[prop];
          }
      }
      return target;
  }
  var o=fn1(obj);
  obj.d1.d2.d3.c4=100;
  console.log(o);
  ```

- ##### 二叉树：

- ```javascript
  var obj={
      left:{
          value:1,
          left:{
              value:3
          },
          right:{
              value:4
          }
      }
      right:{
          value:2,
          left:{
          	value:5
          },
          right:{
              value:6
          }
  	}
  }
  function fn1(o){
      console.log(o.value);
      if(o.left) fn1(o.left);
      if(o.right) fn1(o.right);
      console.log(o.value);
  }
  fn1(obj);
  ```

- ##### 深查找

- ```javascript
  function fn3(o,value){
      for(var prop in o){
          if(o[prop]===value) return prop;
          if(typeof o[prop]==="object" && o[prop]!==null){
              var s=fn3(o[prop],value)
              if(s) return s;
          }
      }
  }
  var prop=fn3(obj,14);
  console.log(prop)
  ```

### 数组：

- ##### 数组是一个列表
- ##### 起始为0；
- ##### 实际上是按照下标存储元素的方式，将值存在列表中。
- ##### `arr[1]===4`
  
  - ###### 1就是下标
  - ###### `arr[1]`下标变量
  - ###### 4就是元素
- ##### 数组创建：
  
  - ###### 字面量创建--`var arr=[1,2,3]`
  - ###### 构造函数创建
    
    - `var arr=new Array(1,2,3)`
    - `var arr=Array(1,2,3)`
  - ###### 对象创建--`var arr=new Object([1,2,3])`
- ##### JS中数组的元素可以类型不同
- ##### 因为空元素就是下标没有对应的值，所以可以通过 下标 in 数组 这种方式判断。
- ##### 如果构造函数中的参数数量大于1个，则所有的参数都是这个数组的元素。
- ##### 如果构造函数的参数只有一个，就是数组的长度
- ##### 如果仅有一个参数，并且是数值类型，但并不是***正整数***，就会报错。
- ##### 如果仅有一个参数，但不是数值类型，则会把这个参数放在数组的第0位。

#### 数组长度

- ##### 元素个数（包括空元素）

- ##### `arr.length`是一个可读可写的属性

- ```javascript
  var arr=[1,2,3];
  arr.length=5;
  arr.length=2;
  console.log(arr);
  ```

- ##### 如果当前数组的长度小于设置的长度，则增加(新长度-原长度)数量的空元素

- ##### 如果新长度小于原长度，则删除(原长度-新长度)数量的元素

- ##### `arr.length=0`清空数组

- ##### `arr.length--`删除尾部的最后一个元素

- ##### length不能小于0或者是负数

- ##### 必须是一个正整数--`arr.length="a"`

- ##### `push`--尾部添加一个或者多个元素,并且返回数组的新长度

- ```javascript
  var arr=[];
  var i=0;
  while(arr.push(i++)<10);
  console.log(arr);
  ```

- ```javascript
  function array_push(arr:Array){
      if(arr==undefined || arr.constructor!==Array) throw new Error("错误参数");
      //throw new Error() 抛出错误，中断后续代码的执行
      if(arguments.length===1) return arr.length;
      for(var i=1;i<arguments.length;i++){
          arr[arr.length]=arguments[i];
      }
      //给数组尾部一旦添加元素，arr.length就会自动重设
      return arr.length;
  }
  var arr=[];
  array_push(arr,2);
  ```

- ##### pop--删除数组尾部的一个元素，并且返回被删除的元素

- ```javascript
  function array_pop(arr){
      var item=arr[arr.length-1];
      arr.length--;
      return item;
  }
  var arr[4,3,2,1]
  var item=array_pop(arr);
  console,log(arr,item);
  ```

- ##### shift--删除数组头部的元素，并且返回被删除的元素

- ```javascript
  function array_shift(arr){
      if(arr.length===0) return;
      var item=arr[0];
      for(var i=0;i<arr.length-1;i++){
          arr[i]=arr[i+1];
      }
      arr.length--;
      return item;
  }
  var arr=[1,2,3,4];
  var item=array_shift(arr);
  console.log(item)
  ```

- ##### unshift--向数组的头部添加一个元素，并返回数组的新长度

- ```javascript
  function array_unshift(arr){
      //控制参数代码
      if(arr==undefined || arr.constructor!==Array) throw new Error("错误参数");
      //throw new Error() 抛出错误，中断后续代码的执行
      if(arguments.length===1) return arr.length;
      var len=arguments.length-1;//当前添加参数数量
      arr.length=len=len+arr.length;//添加元素后数组的新长度
      while(len>0){
          if(len>arguments.length-1){
              arr[len-1]=arr[len-arguments.length];
          }else{
              arr[len-1]=arguments[len];
          }
          len--;
      }
      return arr.length;
  }
  var arr=[1,2,3,4];
  array_unshift(arr,-1,0);
  ```

- ##### `concat`--合并数组，可以将一个数组和另外一个数组合并成一个新数组，原数组不变。

- ##### 可以是用一个数组合并多个元素，产生一个新数组

- ##### 如果不填写参数，则复制原数组，产生一个新数组

- ```javascript
  function array_concat(arr){
      if(arr==undefined || arr.constructor!==Array) throw new Error("不是数组")
      var arr1=[];
      for(var i=0;i<arr.length;i++){
          arr1[i]=arr[i];
      }
      if(arguments.length===1) return arr1;
      for(var j=1;j<arguments.length;j++){
          if(arguments[j] && arguments[j].constructor===Array){
              for(var k=0;k<arguments[j].length;k++){
                  arr1[arr1.length]=arguments[j][k];
              }
          }else
          arr1[arr1.length]=arguments[j];
      }
      return arr1;
  }
  var arr=[1,2,3,4];
  var arr1=array_concat(arr);
  arr[0]=100;
  var arr1=array_concat(arr,1,2,3);
  var arr1=array_concat(arr,[1,2,3]);
  var arr1=array_concat(arr,[1,2,3],[4,5,6]);
  var arr1=array_concat(arr,[1,2,3],0,2,1,[4,5,6]);
  console.log(arr1);
  ```

- ##### `join`--连接字符串，用符号连接数组的元素并且生成字符串

- ```javascript
  function array_join(arr,separator){
      if(arr==undefined || arr.constructor!==Array) throw new Error("不是数组")
      if(separator===undefined) separator=",";
      separator=String(separator);
      var str="";
      for(var i=0;i<arr.length-1;i++){
          str+=arr[i]+separator;
      }
      str+=arr[arr.length-1];
      return str;
  }
  var arr=[1,2,3,4];
  var str=array_join(arr,"");
  console.log(str);
  ```

- `fill`

- ```javascript
  function array_fill(arr,item,start,end){
      if(arr==undefined || arr.constructor!==Array) throw new Error("不是数组")
      if(start===undefined) start=0;
      if(end===undefined) end=arr.length;
      for(var i=start;i<end;i++){
          arr[i]=item;
      }
      return arr;
  }
  var arr=Array(10);
  arr=array_fill(arr,1,2,5);
  console.log(arr);
  ```

- ##### `Array.from`将一个列表转换为数组

- ##### 下标有0，1，2，3，4，5就是有列表，必须要有length。

- ```javascript
  function fn1(){
      var str=arguments.join("");
      console.log(str);
      var arr=Array.from(argumentss);
      var str=arr.join("");
      console.log(str);
  }
  fn1(1,2,3,4);
  ```

- ```javascript
  Array.form_1=function(list){
      var arr=[];
      if(!list.length) return [];
      for(var i=0;i<list.length;i++){
          arr[i]=list[i];
      }
      return arr;
      
      //简单
      return Array.prototype.slice.apply(list);
      return Array.prototype.slice.call(list);
      return [].slice.apply(list);
      return [].slice.call(list);
      return [].concat.apply([],list);
      return Array.prototype.concat.apply([],list);
  }
  ```

- ##### `Array.isArray`判断元素是否为数组

- ```javascript
  Array.isArray_1=function(list){
      return (list && list.constructor===Array) ? true : false;
  }
  ```