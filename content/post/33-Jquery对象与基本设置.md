---
title: Jquery对象与基本设置
date: 2021-04-19 08:01:55
categories:
tags: [JavaScript,Jquery]
---

## Jquery：

1.x 版本支持IE6、7、8低版本浏览器，官方不再更新

### 入口函数：

```html
<script src="../js/jquery.js"></script>
$(document).ready(function(){})
//或者
$(function(){})
```

入口函数可以在一个页面多次使用，表示页面结构加载完成。jQuery中的this指向已经处理，不建议在jQuery下使用箭头函数。

<!--more-->

### jQuery对象与DOM对象：

通过原生JS的`getElementById`等方法获取的元素对象称为DOM对象。使用jQuery的`$`对象获取的元素称为jQuery对象。

```html
//获取DOM对象
let elem = document.getElementById("username");
//转为jQuery对象
console.log($(elem));
//获取jQuery对象
let jelem = $("#username");
//转为DOM对象
$("#username")[0];//$("#username").get(0)
```

### 选择器：

##### 基本选择器

分为全选选择器、ID选择器、类选择器、标签选择器、并集选择器；其中并集选择器==群组选择器。jQuery具备隐式迭代。

在使用并集选择器，进行事件绑定时，箭头函数的指向问题：

```HTML
$('ul,ol').click(ev=>{
	//$(ev.target);
})
```

##### 层级选择器

子代选择器（`$('ul>li')`）、后代选择器（`$('ul li')`）

##### 筛选选择器

`:first`获取该选择器的第一个元素，`:last`获取最后一个元素，`:eq(index)`获取选择器中索引值为index的元素，`:odd`：索引值为奇数的元素，`:even`：索引值为偶数的元素。

> $('li:first')：第一个li元素

### 筛选方法：

- `parent()`查找父类
- `parents()`查找其指定的父类
- `children()`相当于子代选择器
- `find()`相当于后代选择器
- `siblings()`查找元素的兄弟元素，但不包括自己本身
- `nextAll()`查找当前元素之后所有的同辈元素
- `prevAll()`查找当前元素之前所有的同辈元素
- `eq()`相当于筛选选择器中的eq

### 样式设置：

```html
//获取样式:
$(选择器).css('样式名称');
//设置样式:
$(选择器).css('样式名称','值');
//同时设置多个样式
$(选择器).css({'样式名称1':'值1', '样式名称2':'值2'});
//添加样式:
$(选择器).addClass(‘样式名称’);
//删除样式:
$(选择器).removeClass(‘样式名称’);
```

### 属性操作：

##### 元素固有属性：

```html
//获取
$(选择器).prop('属性名')
//设置
$(选择器).prop('属性名', '值')
```

##### 自定义属性：

```html
//获取
$(选择器).attr('属性名')
//设置
$(选择器).attr('属性名', '值')
```

### 元素文本内容：

```html
//HTML内容：
//获取：
$(选择器).html()
//设置：
$(选择器).html('内容')

//文本内容:
//获取:
$(选择器).text()
//设置：
$(选择器).text('内容')

//表单值内容：
$(选择器).val()//获取
$(选择器).val('内容')//设置
```

### 元素操作：

##### 元素遍历

```html
// each方法遍历元素
$(选择器).each(function((index,domEl){})
$.each($(选择器),function((index,domEl){})
```

##### 创建元素

```html
var jqelem = $('<div>你好世界</div>')//创建
```

##### 添加元素

- 添加一个子元素

```html
// 内容放入匹配内容最后面
$(选择器).append(jqelem);
// 内容放在最前面
$(选择器).prepend(jqelem);
```

- 添加一个兄弟元素

```html
// 放到匹配元素的后面
$(选择器).after(jqelem);
// 放到匹配元素的前面
$(选择器).before(jqelem);
```

##### 删除元素

```html
//删除元素本身
$(选择器).remove();
//删除匹配的元素集合中所有的子节点
$(选择器).empty();
$(选择器).html('');
```

[接口文档](https://jquery.cuishifeng.cn)