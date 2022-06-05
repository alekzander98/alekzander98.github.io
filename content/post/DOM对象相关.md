---
title: DOM对象相关
date: 2021-03-26 16:28:01
categories:
tags: [JavaScript]
---

1. DOM节点的遍历
2. DOM中元素与节点
3. 元素的删除与替换

<!--more-->

### DOM:

- 节点遍历和增删查改

- document.body//body

- document.title//title

- document.head//head

- document.documentElement//html标签

- document//是整个文档

- document.URL//location.href

- document.domain//域名

- document.styleSheets//CSS的style列表

- document.body.parentElement//父元素

- document.body.parentNode//父节点

  - 任何元素的父元素和父节点是相同的，除了文本和注释类型

- div.children//子元素 HTML标签

  - 元素就是HTML标签，不包含注释、文本、换行、文档。

- div.childNodes//子节点

- div.firstChild//第一个子节点

- div.firstElementChild//第一个子元素

- div.lastChild//最后一个子节点

- div.lastElementChild//最后一个子元素

- div.previousSibling向上一个兄弟节点

- div.previousElementSibling向上一个兄弟元素

- div.nextSibling向下一个兄弟节点

- div.nextElementSibling向下一个兄弟元素

- > HTMLDivElement-->HTMLElement-->Element-->Node-->EventTarget-->Object

- 判断类型

  - 标签的构造函数
  - 标签的nodeName

- 在Node中分支---文档、元素类型

- 创建容器

- ```javascript
  var elem= document.createElement("标签名")
  var div=document.creatElement("div");
  ```

- ```javascript
  document.body.appendChild(div)
  ```

- ```javascript
  var elem=document.creatDocumentFragment();//创建文档碎片容器
  ```

- 图片的创建

- ```javascript
  var img = new Image();
  var img=document.createElement("img");
  ```

- 元素插入的位置：

- 将某个元素添加再父元素里面的尾部

  - > 父元素.appendChild(子元素)

- ```javascript
  function insertPrev(newElem,targetElem){
      targetElem.parentElement.insertBefore(newElem,targetElem);
  }
  function insertNext(newElem,targetElem){
      targetElem.parentElement.insertBefore(newElem,targetElem.nextSibling);
  }
  
  function warp(newElem,targetElem){
      var parent=targetElem.parentElement;
      var next=targetElem.nextSibling;
      newElem.appendChild(targetElem);
      parent.insertBefore(newElem,next);
  }
  ```

- 元素的复制

- ```javascript
  //新元素=原元素.cloneNode(false) 可以复制原标签的所有属性和样式，不包含内容和后代元素
  var div1=document.createElement("div");
  div1.style.color="red";
  div1.innerHTML="abc";
  document.body.appendChild(div1);
  
  var div2=div1.cloneNode(false);
  document.body.appendChild(div2);
  
  var div5=div3.cloneNode(true);//深复制，可以将内容和后代一起复制
  document.body.appendChild(div5);
  // 所有具有id的标签都需要重新定义id
  node.cloneNode(false) //浅复制
  node.cloneNode(true) //深复制
  ```

- 删除

- ```javascript
  div1.remove();//自己删除自己
  document.body.removeChild(div1);//通过父元素删除子元素
  ```

- 替换

- ```javascript
  //父元素.replaceChild(新元素,要替换元素)
  document.body.replaceChild(div2,div1);
  ```

- `textContent`===获取设置元素的文本节点

- ```javascript
  div1.textContent//所有文本节点及其后代的所有文本节点
  div1.textContent="abc";//将div1中的所有元素替换为这个abc的文本节点
  ```

- 文本节点：txtNode

- ```javascript
  var txtNode=document.createTextNode("文本内容");
  ```

- 案例：

  - 数据驱动显示