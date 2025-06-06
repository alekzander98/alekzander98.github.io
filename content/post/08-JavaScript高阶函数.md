---
title: JavaScript高阶函数
date: 2020-04-06 13:40:48
categories:
tags: [JavaScript, Vue]
---
1.filter函数
2.map函数
3.reduce函数
了解JavaScript中高阶函数的使用开发可以更加简洁
<!--more-->
## 案例
首先定义一个数组：
	const nums = [50, 20, 101, 100, 10, 200, 33, 22]
完成三个需求：
	1.取出所有小于100的数字
	2.将取出的数进行 乘 2 转化
	3.将转化后的所有数字相加,得到最终的结果

### filter函数

解决需求1：
向filter函数传入一个带参数的方法，函数内部会创建一个数组，根据返回的布尔类型的值，把方法中满足条件/返回值为true的加入数组
```bash
let newNums = nums.filter(function (n) {
  return n < 100
})
console.log(newNums);
```
执行结果：
[ 50, 20, 10, 33, 22 ]

### map函数

解决需求2：
map函数对数组中所有内容进行转化
```bash
let new2Nums = newNums.map(function (n) {
  return n * 2
})
console.log(new2Nums);
```
执行结果：
[ 100, 40, 20, 66, 44 ]


### reduce函数的使用

解决需求3：
reduce函数的作用对数组中所有的内容进行汇总
```bash
let final = new2Nums.reduce(function (preValue, n) { //preValue参数表示上一个数
  return preValue + n
}, 0) //这里的0表示reduce函数的初始值/preValue的初始值
console.log(final);
```
执行结果：
270

### 总结

```bash
let final = nums.filter(function (n) {
  return n < 100
}).map(function (n) {
  return n * 2
}).reduce(function (prevValue, n) {
  return prevValue + n
}, 0)
console.log(final);
```
结果同样为：
270

或者使用箭头函数
```bash
let final = nums.filter(n => n < 100).map(n => n * 2).reduce((pre, n) => pre + n);
console.log(final);
```
这样可以一行得到结果，结果同样正确。