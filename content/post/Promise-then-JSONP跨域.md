---
title: Promise.then/JSONP跨域
date: 2021-04-14 09:49:27
categories:
tags: [Server,ES6，异步]
---

## Web Worker

同步实现优先于异步实现

- 异步队列中如果先执行完会等待同步完成
- 子元素和父元素的侦听顺序

Promise.then

GET和POST区别

JSONP跨域

<!--more-->

## Promise：

- ```javascript
  function loadImage(src){
      return new Promise(function(resolve,rejrct){
          var img=new Image();
          img.src=src;
          img.onload=function(){
              resolve();
          }
          img.onerror=function(){
              reject();
          }
      })
  }
  loadImage("./img/1.jpg").then(function(){
      console.log("1加载完成");
      return loadImage("./img/2.jpg")
  }).then(function(){
      console.log("2加载完成");
      return loadImage("./img/3.jpg")
  }).then(function(){
      console.log("3加载完成");
  })
  ```

- resolve和reject两个方法只能二选一执行

- PromiseState初始值：pending;

- 当执行resolve函数时或者reject函数时，首先判断状态是不是pending，如果是pending，则运行调用相应的函数；如果不是pending，不执行后面的函数

- resolve执行：fulfilled

- reject执行：rejected

- 链式结构

- `Promise.all`

- 当列表中每一个Promise全部执行完成resolve后，调用then中的函数，并且将resolve中的参数全部放在一个数组中共同返回

- `promise.race`

- 若干个Promise最先完成执行的返回参数

- resolve和reject只能传一个参数

- 回调函数--红绿灯

- ```javascript
  function showLight(light,time){
      if(!item) time=2000;
      return new Promise(function(resolve,reject){
          setTimeout(function(){
              console.log(light);
              resolve();
          },time);
      })
  }
  fns();
  function fns(){
      showLight("红灯").then(function(){
          return showLight("黄灯");
      }).then(function(){
          return showLight("绿灯");
      }).then(function(){
          fns();
      })
  }
  ```

连缀：

- 在promise.then中如果连缀没有返回promise，会新创建一个，并且执行resolve。

## GET和POST区别：

- 发送数据的大小
  - POST 不限制数据发送大小
  - GET  限制数据发送大小 4KB
- 安全性
  - GET 更为安全的一种请求方式
  - POST 是存在风险的
  - 数据放在请求头里还是放在请求体里差别不大
- 性能
  - POST 性能差
  - GET 性能好
- 语义化差异
  - POST 是INSERT行为
  - GET 是SELECT行为

## 跨域：

同源策略：

- 协议
- 域名
- 端口

限制：

- 浏览器限制

- 仅限于浏览器和服务器交互会发生跨域限制

- 仅限于xhr对象发起请求会有跨域限制

技术：

- JSONP
- CORS
- 服务器代理

JSONP：

- 降权===>只能发送GET请求
- 使用标签代替xhr发送请求
- 标签src可以规避同源策略问题
- 必须提前准备好函数体，配合请求过来的函数调用
- 请求的script标签必须放在全局函数声明之后

优化：

- 动态生成script标签
- 防止冗余标签，加载完成后删除

![状态码](img.png)