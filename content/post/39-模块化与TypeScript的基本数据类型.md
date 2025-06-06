---
title: 模块化与TypeScript的基本数据类型
date: 2021-04-27 22:57:43
categories:
tags: [TypeScript]
---

## 模块化

**方案**：AMD[requirejs]、CMD[seajs]、CommonJS[nodejs]和ES6

*AMD和CMD*，两者的**区别**是前者是对于依赖的模块提前执行，而后者是延迟执行。 前者推崇依赖前置，而后者推崇依赖就近，即只在需要用到某个模块的时候再require。

CMD 有async方法实现异步

<!--more-->

##### AMD

非同步加载模块，允许指定回调函数

```javascript
// 定义模块，没有依赖项
define(function () {
    function fn1() {
        return '你好fn1'
    }
    // 导出模块
    return {
        fn1
    }
})
// 定义模块有这依赖项
define(['m1'], function (m1) {
    function fn2() {
        console.log('m2模块', m1.fn1());
    };
    // 暴露模块
    return { fn2 };
})
// 入口文件
(function () {
    requirejs.config({
        // 配置js路径
        paths: {
            m1: './module/m1',
            m2: './module/m2',
            jquery: './module/jquery'
        }
    });

    requirejs(['m2', ' jquery'], function (m2) {
        m2.fn2();
        $('#box').click(function(){
            console.log(1111);
        })
    })
})()
// 页面文件
<div id="box">点击试一下</div>
<script data-main="./main.js" src="./lib/require.js"></script>
```

##### CMD

```javascript
// 没有依赖的模块
define(function (require, exports, module) {
    function fn1() {
        console.log("m1模块下面的fn1方法");
    };
    // 导出模块
    module.exports = {
        fn1
    }
})
// 有依赖的模块
define(function (require, exports, module) {
    // 导入依赖模块
    let m1 = require('./m1')
    m1.fn1()
    function fn2() {
        console.log('我是m2模块下面的fn2方法');
    }
    exports.fn2 = fn2
});
// 主入口文件
define(function (require) {
    let m2 = require('./m2')
    m2.fn2()
    let jquery = require('./jquery')

    console.log(jquery('body'));
})
// 页面
<script src="./lib/sea.js"></script>
<script>
    seajs.use('./module/main.js')
</script>
```

##### CommonJS

前端浏览器不支持，Nodejs中使用的是这个规范，核心思想就是通过 require 方法来同步加载所要依赖的其他模块，然后通过 exports 或者 module.exports 来导出需要暴露的接口。

```javascript
// 导出
module.exports = 对象
// 导入
const 变量 = require(路径)
```

##### ES6

在ES6中，我们可以使用 import 关键字引入模块，通过 exprot 关键字导出模块，功能较之于前几个方案更为强大，也是我们所推崇的，但是由于ES6模块化目前还无法在所有的浏览器中支持，所以在浏览器中使用需要进行编译处理。目前chrome是支持的。

```javascript
export [default] 对象
import * as obj form '路径'
import xxx form '路径'
import {xx as 别名} from '路径'
```

## TypeScript

TypeScript 是 JavaScript 的超集，扩展了 JavaScript 的语法，因此现有的 JavaScript 代码可与 TypeScript 一起工作无需任何修改，TypeScript 通过类型注解提供编译时的静态类型检查。

![image-20210427192231042](img1.png)下载完nodejs后可以切换npm镜像

> npm install -g cnpm --registry=https://registry.npm.taobao.org

也可以安装nrm--镜像源管理，高版本会报错，安装目录下找到 `cli.js`文件在17行，修改如下：

> const NRMRC = path.join(process.env[(process.platform == 'win32') ? 'USERPROFILE' : 'HOME'], '.nrmrc');

使用npm全局安装TypeScript

> npm i -g typescript
>
> //安装完成查看
>
> tsc -v

通过tsc命令对ts文件进行编译 `tsc app ts`

![image-20210427193235016](img2.png)

### 基本数据类型

类型声明：

```typescript
const app:类型
function fn(p1:类型,p2:类型):类型{}
```

自动类型：

当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型，工作中建议还是手动指明变量类型，尽量少的去让TS自动判断类型。

```typescript
var app = "Hello"
console.log(app);
```

##### 布尔值

```typescript
let isDone: boolean = false;
```

##### 数值

```typescript
let decLiteral: number = 6;           // 十进制
let hexLiteral: number = 0xf00d;     // 十六进制
let binaryLiteral: number = 0b1010;  // 二进制
let octalLiteral: number = 0o744;    // 八进制
```

##### 字符串

```typescript
let name: string = "bob"
let username:string = `aaabbbccc`
```

##### any/unknown

any任意类型, 类型安全的any类型；当前定义的变量，不知道是什么类型。

```typescript
let app:any = 66
let app:unknown = 66
```

##### void

表示没有任何类型，只能赋值为undefined和null，一般常用于返回返回值的声明，如果一个函数没有返回值，就可以用它。

```typescript
let unusable: void = undefined;
function warnUser(): void {
    console.log("This is my warning message");
}
```

##### null和undefined

##### never

表示那些永不存在的值的类型，也在函数执行判断时，永不可能执行到的代码块。

![image-20210427194331157](img3.png)

##### 字面量

可以使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围

```typescript
let color: 'red' | 'blue' | 'green';
let num: 1 | 2 | 3;
```

#### 枚举

使用枚举类型可以为一组数值赋予特定的值。

使用时，如果定义的枚举内部没有指定值，则默认从0开始；如果设置枚举的值为数值，则后面未设置的值向下累加；如果设置的值为字符串，需要全部设置；常量定义的枚举，不可直接打印类型，但是可以通过 `.`读取到里面的值；

```typescript
enum Color {
    Red,
    Green,
    Blue,
}
或
enum Color {
    Red = 1,
    Green = 2,
    Blue = 3,
}
let c: Color = Color.Green;
```

##### Object

```typescript
let obj: object = {}
let obj:{id:number,name?:string}  // ?可选
// id是必须要有的，后面可以任意key，key为字符串，值为任意类型
let obj:{id:number,[props:string]:any}
// 给函数参数定义类型和返回值定义类型
type fnType = (a:number,b:number)=>number
const fn:fnType = (a:number,b:number):number=>a+b
```

![image-20210427195919260](img4.png)

