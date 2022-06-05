---
title: 服务器端与PHP
date: 2021-04-12 20:18:30
categories:
tags: [Server]
---

1. 服务器基础
2. HTTP协议
3. PHP

<!--more-->

## 服务器：

- 局域网IP、公网IP、环回IP

- ##### 内网穿透

- ipconfig

## 本机应用服务器软件：

- 根目录：服务器可以共享的文件--WWW

- WWW之下的文件，子集目录

## HTTP协议：

- 可靠协议：发出去的数据，与接收方一致

- TCP：一人掉线全员等待

- HTTP：可靠协议

### 三次握手：主要是确保链接的可靠性

- 服务器可以接收请求
- 客户端可以发送请求
- 客户端可以接收响应

### 发请求、接响应

- B=>S: request

- S=>B: response

### 四次挥手断开连接：验证数据的完整性

### 四次挥手验证结束之后，本次交互断开连接

- HTTPS: HTTP协议的加密版本!

### HTTP报文：

- ##### 请求报文（request）

- ```js
   GET / HTTP/1.1
    
        Host: www.baidu.com
        Connection: keep-alive
        Cache-Control: max-age=0
        sec-ch-ua: "Chromium";v="88", "Google Chrome";v="88", ";Not A Brand";v="99"
  ```

- 请求行：

  - 请求行（GET|POST|PUT|PATCH|DELETE）
  - 请求头（配置信息）
  - 请求空行
  - 请求体（请求携带的数据）只在POST情况下携带请求体

- ##### 响应报文（response）

- ```javascript
  HTTP/1.1 200 OK
  Bdpagetype: 2
  Bdqid: 0xb9f8696600063622
  Cache-Control: private
  Connection: keep-alive
  ```

- 响应行（协议版本，协议状态码）

- 响应头（响应内容描述）

- 响应体（preview和response中）

### 端口号：

- 计算机的端口 : 允许访问(请求)进入计算机的路径;  65535 个端口; 
- http协议：80
- https协议：443

## 请求携带数据：

- 方式：

- > url?key=value&key2=value

- 使用GET请求携带的数据, 以key为健，以value为值 以组的形式进行发送; 

- 每条数据都以key=value形式进行分割; 

- ##### 我们通常把传递数据的key值称之为 `字段`;

### 同步实现优先于异步实现：

## PHP：

- 使用 `<?php`标识着php代码的开始

- `echo`关键字==翻译：回声===输出字符串

### 字符串输出：

- `‘’`纯字符串；

- `“”`：拼接字符串，可以在字符串里面直接写变量；

- php中没有对象，只有数组

- `.`在运算在php中代表字符串拼接

### 数组：

- ##### 数字数组：下标==数值

- ```php
  $arr=array();
  echo $arr;
  echo var_dump($arr);
  echo $arr[1];
  echo count($arr);
  echo sizeof($arr);
  ```

- echo输出数组会默认调用转换字符串的方法把自己转换为字符串

- 使用 `var_dump()`查看内部的内容和结构

- 数组中使用 `[]`取出内容

- 获取数组的长度===`sizeof/count`

- php一旦被请求，返回结果默认拼接字符串，一次性返回；

- ##### 关联数组：key==value

- ```php
  $assoc_array = array( "key1" => "value1" );
  echo var_dump($assoc_array);
  ```

- 数据关联使用`=>`进行；

### 前后端交互标准结构：JSON

- php将数组转换为符合json格式
- `json_encode()`