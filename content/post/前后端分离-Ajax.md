---
title: 前后端分离/Ajax
date: 2021-04-13 22:18:44
categories:
tags: [Server,Ajax]
---

1. 前后端分离技术
2. PHP与MySQL数据库
3. Ajax--异步

## http协议补充 - 请求携带数据

- GET : GET 请求的数据全部放在url路径之中 ; 
- 路径后方可以拼接php数据 
  - url?key=value&key2=value
  - 使用GET请求携带的数据, 以key为键，以value为值 以组的形式进行发送; 
  - 每条数据都以key=value形式进行分割; 
- 我们通常把传递数据的key值称之为 `字段`;

<!--more-->

## php操作数据库 

- php是一个后端语言， 其核心职责之一就是操作数据库; 

- php本身给我们提供了一套操作数据库的工具 : mysqli 

  - 1. 链接数据库 connect; 

  - ```javascript
    //mysqli_connect("服务器路径" , "数据库账号" , "数据库密码" , "数据库名称")
    ```

  - 返回值有可能为 资源秘钥 ；也可能为空值; 

  - 2. SQL语句执行工具  query ; 

  - ```php
    $insert_sql = "INSERT INTO `usertable` VALUES ('ankang' , '123123' , null)";
    $res = mysqli_query($con , $insert_sql);
    ```

  - 我们操作数据库还是需要sql语句; 

  - 3. php查询数据库会返回一个资源类型，我们会对资源类型进行特殊处理 ; 

    - fetch_all API ; 

    ```php
    $array = mysqli_fetch_all($select_res,MYSQLI_ASSOC);
    echo json_encode($array);
    ```

    - MYSQLI_ASSOC MYSQLI_NUM ;

  - 4. error : 数据库报错信息; 

  - 错误处理判断 `$con`

  - ```php
    if( !$con ){
        // API : die 终止全局代码执行 
        // API : mysqli_error($con) 报出当前连接的错误信息用于排错; 
        die("数据库连接错误,错误信息:" . mysqli_error($con));
    }
    ```

  - ```php
    mysqli_close($con);//关闭数据库
    ```

- 引入连接数据库的文件

- ```php+HTML
  require("./02_php链接数据库.php");
  ```

- 在php之中编写sql语句所有的表名称，以及字段名全都用`` 反引号引起来; 

- 我们表之中设置好类型为varchar的字符串，一定要在数据前面加上 '' ---把数据识别为varchar; 

## 前后端分离 

- 使我们现代项目开发的核心思想; 
- 前后端的开发效率不一致 ： 
  - 后端慢 :  1. 业务逻辑 2. 数据库操作 3. 安全 
  - 前端快 :  

- 前后端职责进行明确的划分 : 

  - 前端工作职责大多在使用浏览器发送请求，处理服务的响应; 
  - 后端的工作职责是做一个黑盒子; 

- 如何协作 : 
  - 在项目开发之前就定义好请求和响应的规则; 
  - 我们需要书面格式的规则说明 => 面向前端的 => 接口文档; 

# 接口文档 

## 接口路径 

- url : http://www.baidu.com/xxx.php

## 请求方式 

- GET | POST | PATCH | PUT | DELETE 

## 请求字段信息 

- username : string 
- password : string 

## 返回数据实例 

```json
{ "data"  : "hello world" }
```


- 注意事项 : 
  - 1. 定义的字段名必须是英文; 
  - 2. 请求方式必须明确; 
  - 3. 后端响应的数据必须是json格式;  
  - "{"username" : "wuyanzu" , "password":123456, "id" : 3 , "type" : "success"}"; 
  - 有一个API 可以把json字符串转换成对象数组类型; 

## AJAX：

- 异步的类型：
  - 事件; => 事件处理函数
  - 定时器/延时器; => 定时器的回调函数
  - ajax
  - promise.then

- 异步 JavaScript and xml

- xhr：（小黄人）XMLHttpRequest

  - 无刷新发起浏览器请求
  - 接收服务器的响应
  - 发起请求--有三步
  - 接收响应--一步

- ```javascript
  //每一个请求都要创建一个新的xhr实例对象
  var xhr=new XMLHttpRequest();
  //请求是必须有目标，并且想要获取数据的
  // - 请求目标
  // - 请求方式
  var url="";
  xhr.open("GET",url)
  //源路径与请求目标的路径同协议、同端口、同域名
  //发送行为
  xhr.send();
  //响应处理
  // - xhr.readyState : xhr的请求状态
  // - 0 ~ 4 五个状态 ; 
  // - 4 : 成功; 
  // - xhr.status : http状态码 ; 
  // 200 : 成功; 
  // 2开头的所有状态码都表示成功; 
  // -xhr.responseText:响应字符串数据
  // -事件：onreadystatechange:判断xhr的状态/http的状态
  console.log(xhr);
  ```

- jsonp：跨域技术

- fetch：高级封装