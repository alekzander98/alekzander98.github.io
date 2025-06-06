---
title: BOM与DOM
date: 2021-03-25 20:11:21
categories:
tags: [JavaScript]
---

1. BOM对象与DOM对象
2. hash与history的区别

<!--more-->

### Data:

- `var date=new Date();`

- 打印时，由对象转换为字符串

- `date.getFullYear()`

- `date.getMonth()`获取月份，0-11，大于11，进位，年增加1；

- `date.getDate()`日期

- `date.getDay()`星期

- `date.getHours()`

- `date.getMinutes()`

- `date.getSeconds()`

- `date.getMilliseconds()`毫秒

- `date.getUTCHours()`国际时间（格林尼治时间）

- `date.getTime()`从1970.1.1.0到现在的毫秒数

  - 时间戳

  - ```javascript
    "地址?time"+new Date().getTime();
    ```

  - 计算时差

  - ```javascript
    timeStart: function () {
        return timeList.push(new Date().getTime()) - 1;
    },
    timeEnd: function (id) {
        if (timeList[id] === undefined) return 0;
        return new Date().getTime() - timeList[id];
    }
    function timeStart() {
        return timeList.push(new Date().getTime()) - 1;
    }
    
    function timeEnd(id) {
        return new Date().getTime() - timeList[id];
    }
    
    var id1 = Utils.timeStart();
    for (var i = 0; i < 100000000; i++) {
    
    }
    var id2 = Utils.timeStart();
    for (var i = 0; i < 100000000; i++) {
    
    }
    console.log(Utils.timeEnd(id1))
    console.log(Utils.timeEnd(id2))
    ```

  - 倒计时

    - 时钟案例

    - 秒表案例

    - ```javascript
      var date=new Date();
      var div1=document.getElementById("div1");
      date.setMinutes(date.getMinutes()+5);
      setInterval(function(){
          var s=parseInt((date.getTime()-new Date().getTime())/1000);
          div1.innerHTML=parseInt(s/60)+":"+s%60;
      },1000);
      ```

- `var date=new *Date*();`

- `date.setFullYear()`设置年

- `date.setMonth()`//0-11 如果大于11就会进位 年增加

- ```bash
  var date=new Date();
  var date1=new Date(date);
  var date1=new Date(date.getTime());
  var date1=new Date(2022,06,05,15,30,20);
  console.log(date1);
  ```

### DOM：

- DOM的节点

  - 标签就是节点，文本、注释、文档、属性
  - document.body.childNodes//所有节点

- 节点获取

  - ID值：一个元素一旦赋值ID默认存在window对象下

  - ```javascript
    var div1=document.getElementById("div1");
    ```

  - 标签名===   `HTMLCollection`

  - ```javascript
    var divs=document.getElementsByTagName("div");
    ```

  - 标签class获取元素列表===`HTMLCollection`

  - ```javascript
    var divs=document.getElementsByClassName("divs");
    ```

  - input标签的name值获取NodeList列表

  - ```javascript
    var inputs=document.getElementsByName("sex");
    ```

  - document.querySelector()//根据选择器获取单个标签

  - document.querySelectorAll()//根据选择器获取多个标签的列表NodeList

  - ```javascript
    var div=document.querySelector("#div1");
    var div=document.querySelector(".divs");
    var div=document.querySelector("div>div");
    var div=document.querySelector("div.div1");//div下子元素.div1
    var div=document.querySelector("div .div1");//div下所有.div元素
    var div=document.querySelector("[type!=text]");
    ```

### BOM：

- window对象

  - `open()`弹出窗口
  - `close()`
  - `innerWidth/innerHeight`窗口中文档的宽高//**只读**
  - `outerWidth/outerHeight`窗口的宽高//**只读**
  - `screenX/screenY`窗口左上角坐标
  - `screenTop/screenLeft`窗口左上角坐标

- #### location对象

  - `reload()`重载，刷新当前页面

  - `location.href=""`跳转网页

    - href是一个可读可写的属性
    - 中文使用URI编码格式

  - `location.assign=""`跳转网页

    - assign是个方法，只能执行

  - `location.replace=""`替换网页，没有历史记录

  - #### location.hash

    - 地址#号后内容
    - 锚点：
      - 标签的ID
      - 超链接a标签的name
    - 区分当前访问的内容，同一个页面，hash不一样，可能不一样

  - location.search

    - ？号后的内容，添加会刷新页面

  - location.host//返回当前 URL 的主机名称和端口号

  - location.pathname//返回当前路径

  - location.port//返回当前 URL 的端口号

  - location.protocol//协议

  - location.origin//返回URL的协议,主机名和端口号

- #### hash与history

  - hash可以创建历史记录，并且在地址栏中增加#内容

  - 超链接设置name跳转

  - ***hash增加时不刷新页面***

  - ```bash
    <div id="div1"></div>
    var div1=document.getElementById("div1");
        div1.innerHTML=Date.now();
        document.onclick=function(){
            location.href=location.href+"#abc";
            // location.href=location.href+"?time="+Date.now();
            // location.href="./a.html";
    }
    ```

  - history.back();回退历史记录

  - history.forward();向前历史

  - history.go();

    - -1：回退
    - 1：前进
    - 0：刷新页面

  - ##### history.pushState();插入状态

  - ##### history.replaceState();替换状态

- #### window.onpopstate事件：当浏览器产生前进回退时被激活，***history***查看历史记录state数据变化

- #### hash用 window.onhashchange事件：表示hash发生变化

- screen对象

  - `screen.availWidth/availHeight`//屏幕中不包含任务栏的宽高
  - `screen.width/height`//全屏幕的宽高

- navigator对象

  - `navigator.userAgent`//返回由客户机发送服务器的user-agent 头部的值
  - `navigator.appName`//返回浏览器的名称
  - `navigator.appVersion`//返回浏览器的平台和版本信息
  - `navigator.appCodeName`//返回浏览器的代码名
  - `navigator.platform`//操作系统平台
  - `navigator.getCurrentPosition(fn,fn)`获取经纬度位置
