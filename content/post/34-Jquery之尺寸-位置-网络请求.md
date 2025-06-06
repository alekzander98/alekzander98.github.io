---
title: Jquery之尺寸/位置/网络请求
date: 2021-04-20 19:40:56
categories:
tags: [JavaScript,Jquery,深复制/浅复制]
---

## Jquery：

#### 尺寸

获取元素的宽高：width()--返回数值类型

- 当有参数时（数字）表示设置宽高

包含内边距：--只能获取，返回数值类型，不能设置

- innerWidth()

包含边框和宽高（如果参数为true->表示包括外边距）：

- outerWidth()

#### 位置

获取、设置元素相对于文档的偏移坐标和父级没有关系，返回为对象{top,left}，值为数值；设置时参数为对象类型

- offset()

获取、设置元素被卷去的头部和左侧距离

- scrollTop([数字])/scrollLeft([数字])

<!--more-->

```javascript
//首先设置window的监听
$(window).scroll(function(){
    //滚动的距离获取文档对象
    console.log($(document).scrollTop());
    //设置
    //$(document).scrollTop(0);
})
```

相对于带有定位的父级偏移坐标，如果父级没有定位，则以文档为准，值为对象

- position

#### extend()

对象浅或深拷贝

```javascript
$.extend([deep(true/false[默认])],target,source1,[scource2...]);
//浅复制：只复制引用类型的第一层基本类型
```

#### 网络请求

```javascript
$.get(URL,callback);//get请求
$.post(URL,data,callback);//post请求
$.ajax({//标准写法
    url:’请求地址’,
    type:’GET/POST/PUT/DELTE’, // 请求http方法
    timeout:10,// 请求超时时间（毫秒）
    headers:{},// 请求添加的额外头信息
    data:{}/string, // 请求体数据
    dataType:’json/jsonp’,// 服务器返回数据类型
    success:function(ret){},// 请求成功回调
    error:function(){},// 请求失败回调
})
```

Promise.all()、Promise.race()

race：只要有一个执行完成，不管执行成功或者失败，程序结束；all：等待所有的promise完成，程序才结束，并且所有的promise一个都不能有错。

```javascript
let p1 = $.ajax({
    url:"请求地址",
    type:"请求方式",
    dataType:"服务器返回数据类型",
})
...
Promise.all([p1,...,pn]).then((r1,...,rn)=>{
    let arr = [r1.data,...,rn.data];
    console.log(arr);
})
```

#### 第三方插件

[插件](http://www.htmleaf.com/jQuery/pubuliuchajian/)

#### 自定义插件

```javascript
//给原型添加方法
$.fn.方法 = function(){}
//
(function($){})(jQuery);
```