---
title: cookie/localstorage
date: 2021-04-16 10:04:23
categories:
tags: [Server]
---

## cookie:

会话跟踪技术

- 会默认在请求头里面携带

- ##### 前端cookie

- 一套协议域名端口对应一套cookie

- 存储在当前浏览器独立创建的本地文件

- cookie里面存储的信息是纯文本形式的

- cookie里面存储信息最大4kb，最大条数为50

- 时效性：默认 session

- cookie是必须同域名，同协议，同端口的页面才可以访问

- 浏览器通过 协议+域名+端口作为key值，cookie作为value，实现映射结构的数据存储

- cookie是有安全限制; (很像作用域 / 以文件夹为单位的父子级包裹关系，决定了cookie的可访问性)

- cookie的所有特征都是用字符串去描述的

<!--more-->

- 获取cookie：`document.cookie`

- ```javascript
  document.cookie = "username=123456";
  ```

- domain 一般不设置;

  - 早期用来做跨域

- cookie设置时一条cookie的所有信息都放在一个字符串上;

- 需要设置path

  - 不同path的cookie不是同一条cookie;

- 设置expires : 过期时间;

- ```javascript
  var d = new Date();
  d.setDate( d.getDate() + 15 );
  // console.log(d);
  document.cookie = "username=456789;path=/GP23;expires=" + d;
  ```

- cookie获取：

- 取出的cookie都用 ;空格 去进行间隔;

- `console.log(document.cookie);`

- ##### 后端cookie

- ```\
  setCookie  设置cookie;
  $_COOKIE   获取cookie; 
  ```


## localstorage：

在当前浏览器，持久存储数据的技术我们称之为本地存储技术

- ##### 特定于页面的协议

- ##### `localStorage` 中的键值对总是以字符串的形式存储。

- 键值对总是以字符串的形式存储意味着数值类型会自动转化为字符串类型

```javascript
localStorage.setItem();
localStorage.getItem();//读取
localStorage.key();//返回数据中某个key值（下标）
localStorage.removeItem();//移除
localStorage.clear();//移除所有
```

## JSON.stringify()：

```javascript
JSON.stringify(value[, replacer [, space]])
```

将一个 JavaScript 对象或值转换为 JSON 字符串，如果指定了一个 replacer 函数，则可以选择性地替换值，或者指定的 replacer 是数组，则可选择性地仅包含数组指定的属性。

## HTTP协议：

无论请求还是响应格式都是http规定的格式

请求报文：

- 请求行：
  - http协议版本：1.1
  - 请求方式：GET   POST    PATCH    PUT    DELETE
- 请求头：
- 请求空行
- 请求体（请求方式为POST）

请求字段：

- GET：放在url之中

- POST：放在请求体之中

响应报文：

- 响应行
  - http协议版本 / 响应状态码
  - 1* : 准备状态 用不着看不到的状态; 
  - 2 : 成功状态; 
  - 3 : 重定向状态;
  - 4 : 请求错误 ; 
- 响应头
- 响应体
