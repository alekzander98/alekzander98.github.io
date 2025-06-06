---
title: TypeScript数据类型/类/泛型
date: 2021-04-28 21:49:19
categories:
tags: [TypeScript]
---

## TypeScript

数组、元组、自定义、联合类型；断言；TS编译配置；TS类概念；泛型

<!--more-->

##### 数组

```typescript
//数字类型的数组
/* var arr: number []
arr=[1,2, 3] */
//字符串数组
/* var arr: string [ ]
arr = ['a', 'b'] */
//泛型定义
// var arr: number[]
var arr: Array<number>
    arr=[1,2,3]
// var arr: string []
var arr: Array<string>
//自定义类型
type userType= { 
    id: number,
    name: string
}
var arr: Array<userType>
arr=[
    {
        id:'1' ,
        name :'张三'
    }
]
```

##### 元组

元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。比如， 你可以定义一对值分别为string和number类型的元组。

```typescript
//定义 tuple---数组的另一种表现形式
//定义一个数组,而且指定它的长度和每一项类型
var arr:[number,string,boolean]
arr=[1,'a',true]
//数组
//var arr1=[1,2]
```

##### 自定义类型

默认类型不满足业务中变量定义，为了更好的后续维护。

```typescript
type url = string
let httpurl:url
httpurl = 'http://xxx.com'
type User = {
    name:string,
    age:number
}
let user:User
user = {name: 'aa',age:20}
```

定义对象类型

```typescript
type userObjectType = {
    id: number,
    name: string
}
var userObj: userObjectType
userObj = {id:1,name:'aaa'}
```

定义数组对象类型

```typescript
var userArr: userObjectType[]
var userArr: Array<userObjectType>
    userArr = [
        { id: 1, name: 'aaa' }
    ]
```

限制函数的参数和返回值

```typescript
type fnType = (id: number, name: string) => string
type fnType = {
    (id: number, name: string): string
}
var fn: fnType = (id: number, name: string): string => {
    return 'aa'
}
fn(1, 'aa')
```

##### 联合类型

```typescript
type userObjectType = {
    // 联合类型
    id: number | string,
    name: string
}
var userArr: Array<userObjectType>
    userArr = [
        { id: 1, name: 'aaa' },
        { id: '2', name: 'aaa' },
    ]
var fn = (id: number | string): void => {
    console.log('qqqq')
}
fn(1)
fn('2')
```

##### 断言

感叹号 `！`当一个对象null或者undefined时，停止；问号`?`可以存在可以不存在。

```typescript
var str: string | number
//str = 'aaa';
console.log((str as string).length)
console.log((<string>str).length)
// dom获取
var dom: any = document.getElementById('root')
// 在实际工作中，有没有可能，dom获取不到，null
dom.style.color = 'red'
```

### 编译配置

[配置文档](https://www.tslang.cn/docs/handbook/compiler-options.html)

tsc编译是可以通过 `tsconfig.json`来进行自定义配置的---通过 `tsc --init`命令生成，当然为了实现实时编译可以在当前目录`npm init -y`生成一个npm命令文件(package.json)，添加一个实时编译命令 `npm run dev`

![image-20210428194846907](img1.png)

![image-20210428194919980](img2.png)

### 类

TypeScript 是面向对象的 JavaScript。类描述了所创建的对象共同的属性和方法。TypeScript 支持面向对象的所有特性，比如 类、抽象类、接口等。

##### 定义类

TS中属性的修饰符有三种：public、protected、private；public：默认值，可以在类、子类和对象中修改；protected：可以在类、子类中修改，受保护的；private：可以在类中修改，私有的。

成员属性名后加!表示成员属性不为null。

readonly：只读--针对成员属性，设置后，可以直接定义时赋值或在构造函数中赋值是不会报错的。

```typescript
class 类名 {
    属性名: 类型;
    //静态属性
    static 属性名:类型
    //只读属性：只针对成员属性
    readonly 属性名:类型 = 值
    // 构造方法
    constructor(参数:类型){
        this.属性名 = 参数;
    }
    //实例方法
    方法名(){}
}
class Myclass {
    public id: number
    constructor(id: number) {
        this.id = id;
    }
    fn1() {
        console.log(this.id)
    }
}
class Child extends Myclass {
    constructor(id: number) {
        super(id)
    }
    childFn2(): void {
        // console.log(this.id)
    }
}
```

##### 属性存取器

对于一些不希望被任意修改的属性，可以将其设置为private，直接将其设置为private将导致无法再通过对象修改其中的属性，我们可以在类中定义一组读取、设置属性的方法，这种对属性读取或设置的属性被称为属性的存取器。

```typescript
class Person{
    private _name: string;
    constructor(name: string){
        this._name = name;
    }
    // 获取器
    get name(){
        return this._name;
    }
    // 修改器
    set name(name: string){
        this._name = name;
    }
}
```

获取器/修改器 ES6中也存在（set/get）

##### 继承

通过继承可以将其他类中的属性和方法引入到当前类中

```typescript
class A {}
class B extends B {}
class Parent1 {
    /* public name:string;
  constructor(name:string){
    this.name = name;
  }*/
    constructor(public name: string) { }
}
class Child1 extends Parent1 {
    constructor(public id: number, name: string) {
        super(name)
    }
}
```

##### 抽象类

抽象类不能被实例化，只能被继承；给类中的方法定义规范；抽象类中可以没有抽象方法，抽象方法如果存在，在继承它的类中一定要实现，否则编译报错。

```typescript
abstract class A{
    // 抽象方法
    abstract run(): void;
    fn(){
        console.log('AAA');
    }
}

class B extends A {
    // 实现抽象方法
    run(){
        console.log('冲起来~');
    }
}
```

##### 接口

主要负责定义一个类的结构，类要实现此接口后，此类只有包含接口定义的所有属性和方法才能定义成功。关键词：`interface/implements`。

类接口

```typescript
interface Person {
    //name属性必须实现
    name: string;
    //可以有，可以没有
    id?: number
    say():void;
}
class Student implements Person{
    constructor(public name: string) {}
    say() {
        console.log('大家好，我是'+this.name);
    }
}
```

函数接口

```typescript
// 定义接口
interface searchInterface {
    (kw: string, page?: number): string[]
}

// 使用
const searchFn: searchInterface = (kw:string):string[] => ['aaa', 'bbb']
```

### 泛型

定义一个函数或类时，有些情况下无法确定其中要使用的具体类型（返回值、参数、属性的类型不能确定）, 此时泛型便能够发挥作用。

##### 泛型函数

在没有没学过泛型之前可以使用，但是这样使用any会关闭TS的类型检查，其次这样设置也不能体现出参数和返回值是相同的类型

```typescript
function test(arg: any): any{
    return arg;
}
```

使用泛型

```typescript
function test<T>(arg: T): T{
    return arg;
}
//这里的<T>就是泛型；T是我们给这个类型起的名字（不一定非叫T），设置泛型后即可在函数中使用T来表示该类型；所以泛型其实很好理解，就表示某个类型；
//直接使用
test(10)
//指定类型
test<number>(10)
//函数中声明多个泛型
function test<T, K>(a: T, b: K): K{
    return b;
}
test<number, string>(10, "hello");
```

##### 泛型类

```typescript
class MyClass<T>{
    num!: T;
constructor(num: T){
    this.num = num;
}
}
const c = new MyClass<number>(10)
// 泛型继承
interface MyInter{
    length: number;
}

function test<T extends MyInter>(arg: T): number{
    return arg.length;
}
```