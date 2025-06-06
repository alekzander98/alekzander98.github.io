---
title: history与hash实现历史记录
date: 2021-03-25 20:18:20
categories:
tags: [JavaScript,历史记录]
---

1. 历史记录的回退与记录
2. hash与history

<!--more-->

```html
<script>
    var list = {
        vegetables: [
            "大白菜",
            "娃娃菜",
            "油麦菜",
            "菠菜",
            "莲藕",
            "春笋",
            "韭菜",
            "胡萝卜",
            "大葱",
        ],
        fruit: [
            "苹果",
            "西瓜",
            "柚子",
            "梨",
            "香蕉",
            "樱桃",
            "草莓",
            "榴莲",
            "桃子",
            "杏",
            "橘子",
        ],
        oil: [
            "大米",
            "小米",
            "黄米",
            "黄豆",
            "绿豆",
            "红豆",
            "精面",
            "莜面",
            "花生油",
            "大豆油",
            "橄榄油",
        ],
        meat: [
            "牛肉",
            "羊肉",
            "鸡肉",
            "猪肉",
            "鸭肉",
            "鱼肉",
            "驴肉",
            "鸡蛋",
            "鸭蛋",
        ],
    };
</script>
```

### history历史记录：

```html
<ul class="menu">
    <li id="vegetables">蔬菜</li>
    <li id="fruit">瓜果</li>
    <li id="oil">粮油</li>
    <li id="meat">禽肉</li>
</ul>
<div id="div1"></div>
```

```javascript
var prev,lis,div1;
init();
function init(){
    div1=document.getElementById("div1");
    lis=Array.from(document.querySelectorAll("li"));
    lis.forEach(function(item){
        item.addEventListener("click",clickLiTab);
    })
    window.onpopstate=popChange;
}
function popChange(e){
    if(!e.state) return;
    changeTab(document.querySelector("#"+location.hash.slice(1)));
    render(e.state);
}
function clickLiTab(){
    changeTab(this);
    var data=list[this.id];
    render(data);
    history.pushState(data,this.id,"#"+this.id);
}
function changeTab(elem){
    if(prev) prev.style.backgroundColor="skyblue";
    prev=elem;
    prev.style.backgroundColor="darkblue";
}
function render(data){
    div1.innerHTML="<ul>"+data.reduce(function(value,item){
        return value+="<li>"+item+"</li>";
    },"")+"</ul>"
}
```

### hash历史记录：

```html
<ul class="menu">
    <li id="vegetables"><a href="#vegetables">蔬菜</a></li>
    <li id="fruit"><a href="#fruit">瓜果</a></li>
    <li id="oil"><a href="#oil">粮油</a></li>
    <li id="meat"><a href="#meat">禽肉</a></li>
</ul>
```

```javascript
var div1,prev;
init();
function init(){
    div1=document.querySelector("#div1");
    window.onhashchange=hashChange;
}

function hashChange(){
    var id=location.hash.slice(1);
    changeTab(id);
    var data=list[id];
    render(data);
}

function changeTab(elem){
    if(prev) prev.style.backgroundColor="skyblue";
    prev=document.querySelector("#"+elem);
    prev.style.backgroundColor="darkblue";
}

function render(data){
    div1.innerHTML="<ul>"+data.reduce(function(value,item){
        return value+="<li>"+item+"</li>";
    },"")+"</ul>"
}
```