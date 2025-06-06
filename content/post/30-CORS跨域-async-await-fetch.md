---
title: CORS跨域/async-await/fetch
date: 2021-04-15 09:58:12
categories:
tags: [Server,异步,Ajax]
---

## CORS跨域：

- CORS跨域是后端响应头的配置
- CORS跨域主要是服务端配置的
- 在后端语言之中配置

## promise:

##### 异步：

- 主要用来解决异步回调地狱问题

- Promise:承诺

- ##### 状态：

  - pendding：正在进行；
  - fulfilled：成功履行承诺
  - rejected：没有遵守承诺

- 承诺的状态一旦发生改变，那么这个状态不可逆

<!--more-->

- 创建：

- ```javascript
  var promise = new Promise(function(fulfill,reject){
      // fulfill 是改变promsie对象状态为成功的工具函数
      // reject   改变promise对象状态为失败的工具函数
      //fulfill();
      //reject();
      setTimeout( function(){
          fulfill();
      } , 3000)
  })
  console.log(promise)
  ```

- 内部工具函数的执行是同步的，只要promise对象被创建出来，函数一定会被执行

- 一般会主动定义promise对象状态改变的规则

- 使用异步程序改变promise状态

- promise对象创建 : 

  - 调用 Promise构造函数; 
  - 传入 resolver 函数; 
  - 在 resolver 函数之中定义状态改变规则; 

- ##### 状态监听函数：

- - `then()`监听promise状态的改变；
  - `catch()`监听promise状态改变为rejected；

  ```javascript
  p.catch( function( err ){
      console.log(err);
  })
  try{
      throw "hello error";
  }catch( e ){
      console.log(e);
  }
  ```

  - `finally()`：只要状态改变了就会进入这个函数中；状态改变不做数据的处理

```javascript
var p = new Promise( function( fulfill , reject ){
    fulfill( "hello world" );
})
p.finally(function( res ){
    // res : 是undefined; 
    console.log("状态改变就会执行" , res);
});
```

- ##### then( fn1 , fn2 )：

- fn1：成功的时候会调用的回调函数

- fn2：失败的时候会调用的回调函数

- ```javascript
  promise.then(function(){
      console.log("状态变为成功");
  },function(){
      console.log("状态变为失败");
  })
  ```

- ##### then方法的返回值：

- 回调函数中没有写return的情况下，返回值为promise对象

- 函数中写了return，新的promise对象的情况下，返回值就是新的promise对象

- 注意：

- 使用then之后可以去返回一个新的promsie对象用以连缀

## async-await:

##### async:

- 用于定义函数的关键字--异步

- ```javascript
  async function load(){}
  ```

- async定义的函数默认的返回值是promise对象

- 只是返回一个promise对象，不能取代promise对象的构造

##### await:

- 只能在async定义的函数中使用
- 会等待promise对象状态变为成功，并且把then里面的参数返回出来
- 将异步回调函数写法变为了同步的代码写法
- await之前的代码都是同步程序
- await后面的所有程序都是异步的

## fetch:

xhr 请求发送工具

- 属于浏览器的高级封装

- 完全不兼容IE---PC端不能用

- ```javascript
  fetch(url,options)
  ```

- 返回值为promise

- ```javascript
  async function load(){
      // 返回响应对象; 
      let response = await fetch("./01_cors.php");
      // 处理响应对象，决定响应数据的类型; 
      let data = await response.text();
      console.log(data);
  }
  load();
  ```