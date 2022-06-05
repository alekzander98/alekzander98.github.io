---
title: Promise中的then方法理解
date: 2021-04-17 08:06:01
categories:
tags: [JavaScript,ES6,异步]
---

## Event Loop:

同步任务、异步任务

##### 宏任务：

- setTimeout

- setInterval

##### 微任务：

- Promise

<!--more-->

## Promise：

先执行同步任务，当执行到宏任务时，开辟**新任务列**放在最顶端执行；当遇到微任务时，放在当前任务列的最低端执行。

```javascript
new Promise (function (resolve,reject) {
	resolve();
}).then(function () {
    setTimeout (function () {
    console. Log("a");
}，0);
});
setTimeout(function () {
    new Promise (function (resolve, reject) {
    resolve();
}). then(function () {
	console.log("b");
});
}, 0);//b,a
```

Promise.then()方法提供一个供自定义的回调函数，如果上一个函数的返回值非函数，则会忽略当前的then方法，回调函数结束会把程序的返回值传给下一个then方法中的函数当作参数使用。没有返回值默认为undefined。

```javascript
let func = function() {
    return new Promise((resolve, reject) => {
        resolve('返回值');
    });
};

let cb = function() {
    return '新的值';
}
```

首先定义了两个函数，对不同情况进行分析：

```javascript
func().then(function () {
    return cb();
}).then(resp => {
    console.warn(resp);
    console.warn('1 =========<');
});
//新的值
//1 =========<
```

`return cb()`表示执行cb函数，并将其结果（新的值）返回出去，在下一个then方法中resp接收了上一个then方法返回值的值（新的值）作为自己回调函数的参数使用。

```javascript
func().then(function () {
    cb();
}).then(resp => {
    console.warn(resp);
    console.warn('2 =========<');
});
//undefined
//2 =========<
```

因为第一个then方法中只是执行了cb方法，而没有return返回值，默认下一个then方法的参数为undefined。

```javascript
func().then(cb()).then(resp => {
    console.warn(resp);
    console.warn('3 =========<');
});
//返回值
//3 =========<
```

第一个then方法中`cb()`函数先执行，结果为（新的值）非函数，官方文档中要求若传入then方法的参数不是函数，当前then方法必须被忽略：

```javascript
func().then(resp => {
    console.warn(resp);
    console.warn('3 =========<');
});
```

所以func()的返回结果直接传给then()方法中的回调函数作为参数resp的值

```javascript
func().then(cb).then(resp => {
    console.warn(resp);
    console.warn('4 =========<');
});
//返回值
//4 =========<
```

关键在于第一个then方法的参数为`cb`=== `then(function(){return '新的值'})`；与第一个实例有同样的效果，同时也满足then方法中要传入一个函数的要求。