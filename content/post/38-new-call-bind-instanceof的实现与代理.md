---
title: new/call/bind/instanceof的实现与代理
date: 2021-04-26 22:53:00
categories:
tags: [JavaScript,ES6]
---

## 类

##### 重写new

new内部过程：创建一个实例对象，this指向当前这个实例对象，会自动把函数执行，但是this内部指向变为实例对象，返回值没有或者是基本值，则返回实例对象；如果是引用值，以定义好的为主。

<!--more-->

```javascript
function Fn() {
    // 创建一个实例对象
    // this指向实例对象
    // 也会像普通函数执行一样，让其执行，只不过this指向实例对象
    // 返回值没有或基本值，则返回实例对象，如果引用值，以自己的为主
}
let f1 = new Fn()
// ===============================================================
function _new(Construct, ...args) {
    // 创建一个实例对象（创建Construct类的实例，让其 对象.__proto__ = Construct.prototype）
    // obj.__proto__ = Construct.prototype
    let obj = Object.create(Construct.prototype)
    // 把函数执行，让this指向实例对象
    let ret = Construct.call(obj, ...args)
    //  处理返回值，引用类型,直接返回引用类型的值
    if (ret !== null && /^(object|function)$/.test(typeof ret)) {
        return ret
    }
    return obj
}

function Fn(name) {
    this.name = name
    this.age = function () {
        console.log('方法===' + this.name)
    }
}
let f1 = _new(Fn, '张三')
f1.age()
```

##### 重写call和bind

```javascript
Function.prototype.myCall = function (ctx, ...params) {
    // 参数可以为 undefined或null
    ctx = ctx == null ? window : ctx
    // 需要保证ctx必须是对象类型的值:因为只有对象才能设置属性
    ctx = !/^(object|function)$/.test(typeof ctx) ? Object(ctx) : ctx
    let self = this
    let ret = null
    // 新增的属性名保证唯一性，防止污染原始对象中的成员数据
    let functionName = Symbol('functionName')
    // 给对象添加属性
    ctx[functionName] = self
    // 执行方法
    ret = ctx[functionName](...params)
    // 删除自定义属性
    delete ctx[functionName]
    return ret
};
function fn(x, y) {
    console.log(this, x, y)
}
let obj = {
    name: '张三'
}
fn.myCall(obj, 2, 3)
```

##### 重写bind

```javascript
Function.prototype.myBind = function myBind(ctx, ...params) {
    let self = this
    return function (...args) {
        self.apply(ctx, [...params, ...args])
    }
};
var obj = { id: 1 }
function fn(...args) {
    console.log(this, args)
}
btn.onclick = fn.bind(obj, 1, 2)
```

##### 重写`instanceof`

不能检测基本数据类型,检测的实例必须是对象；

检测原理：构造函数`Symbol.hasInstance`属性，检测构造函数的prototype是否出现在实例的`__proto__`上

```javascript
function _instanceof(obj, FC) {
    if (typeof FC !== "function") {
        // 抛异常
        throw new Error('类型不能，无法使用')
    }
    if (obj == null) return false
    // 检查是否有Symbol.hasInstance属性
    if (typeof Symbol !== "undefined") {
        let hasIns = FC[Symbol.hasInstance]
        if (typeof hasIns === 'function') {
            // 执行返回true/false
            return hasIns.call(FC, obj)
        }
    }
    // 不支持Symbol
    // 获取类的原型
    let prototype = FC.prototype
    // 获取实例的原型
    let proto = Object.getPrototypeOf(obj)
    // 如果类没有prototype则直接返回false
    if (!prototype) return false
    while (1) {
        // 找到原型链最顶，还没有找到返回false
        if (proto === null) return false
        // 在原型上找到返回true
        if (proto === prototype) return true

        proto = Object.getPrototypeOf(proto)
    }
}
let res = _instanceof(1, Array)
console.log(res)
```

##### 代理拦截

`Object.defineProperty`：ES5方法，该方法可以在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回这个对象。但是无法监听数组变化。其中属性 `enumerable`不可枚举；`configurable`是否可以删除；`value`属性值；`writable`属性是否可修改。

`Proxy`：ES6提供的新的api，Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

- Proxy可以直接监听对象而非属性。
- Proxy直接可以劫持整个对象,并返回一个新对象,不管是操作便利程度还是底层功能上都远强于Object.defineProperty。
- Proxy可以直接监听数组的变化。
- 有多达13种拦截方法,不限于`apply、ownKeys、deleteProperty、has`等等是Object.defineProperty不具备的。

![image-20210426230342845](image1.png)

![image-20210426230349251](image2.png)

#### Reflect

Reflect对象与Proxy对象一样，也是ES6为了操作对象而提供的新API。Reflect对象的设计目的有这样几个。

- 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。
- 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
- 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
- Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

它拥有的API和Proxy一一对应。

`Object.defineProperty:enumerable`