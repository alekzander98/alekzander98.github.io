---
title: JavaScript--对象和函数
date: 2021-03-18 16:44:01
categories:
tags: [JavaScript,深复制/浅复制,垃圾回收机制]
---

1. JavaScript对象相关
2. 垃圾回收机制、内存泄漏
3. JavaScript函数相关

<!--more-->

### 对象：

#### 创建：

1. ##### 构造函数创建法：`var o=new Object();`

2. ##### 字面量创建法：`var o={a:1};`

3. ##### {key:value}==> 键值对的形态

   - ###### 每个键对应唯一的值

   - ###### 表现形式：

     1. `console.log(o.a); `
     2. `console.log(o["a"]);`
     3. 键必须是字符串或者 `symbol`，如果不是则会隐式转换为字符串。

#### 注意：

- ```javascript
  var d={a:1};
  o[d]=5;
  console.log(o[{b:3}]);//5
  //说明：d在括号中没有加引号，所以传入的是d变量（{a:1}）即o[{a:1}]=5;
  //{a:1}在括号中隐式转换为字符串==> [object Object]
  //证明：console.log(String({a:1}),String({b:3}));==> 结果都是[object Object]
  ```

- ```javascript
  var a[];
  var b["c"];
  o[a]=10;
  o[b]=20;
  console.log(o[""],o["c"]);
  //空数组在隐式转换为字符串时，转换为空字符串
  ```

#### 存储：

- ##### 对象的存储一共有两种情况，一种是存储数据，一种是存储函数。

- ##### 存储数据的叫做对象的属性，存储函数的叫做对象的方法。

#### 遍历：

- ##### `for...in...`

  - ```javascript
    //如果键是一个变量，不能使用.语法，必须使用[]带入变量。
    //o.a=10;
    //a["a"]=10;
    for(var prop in o){
        consolie.log(prop,o[prop]);
    }
    ```

  - ```javascript
    o.prop;
    o["prop"];
    for(var prop in o){
    	console.log(prop,o[prop]);
    }
    ```

- ##### `fromCharCode();`

  - ```javascript
    var i,key;
    var o={};
    var j=1;
    for(i=97;i<=122;i++){
        key=String.fromCharCode(i);
        o[key]=j;
        j++;
    }console.log(o);
    ```

#### 删除属性：

##### `delete o.e;`如果删除的属性不存在也不会报错

#### 特点：

1. ##### 对象中存储的内容，相互之间是没有关联的

2. ##### 如果需要再对象中查找是否有某个键 `"a" in o;`

3. ##### 如果需要再对象中查找是否有某个值，只能通过 `for...in...`遍历对象查看每个值是否相等

   - ```javascript
     for(var prop in o)
     	if(o[prop]===3)
     	console.log(prop,o[prop]);
     ```

4. ##### 如果需要根据某个属性，查找对应的值时，速度非常快，删除和添加的速度也非常快。

5. ##### 对象存储在堆中

   - ```javascript
     var o={a:1};
     console.log(o);
     o.a=2;
     ```

     - ##### 浏览器控制台先显示为{a:1}; 再点击查看对象内容时，发生了变化。

### 垃圾回收机制：

- ##### 只有存储引用地址的变量才需要设置null做垃圾标识

- ##### 变量o引用堆中一个对象：将栈中变量o设置为null，表示不再引用对象，然后将堆中这个对象的引用列表中这个变量去除。

- ##### o=null;

- ##### 如果说一个对象被多个变量引用，只有将所有引用对象的变量全部设置为null，才可以将垃圾回收。

- ![image-20210318141645809](image-20210318141645809.png)

- ```javascript
  var o={a:1};
  var o1=o;
  o1.a=10;
  console.log(o.a);
  ```

  - ##### 这种方式是赋值了引用地址，因此o和o1是同一个引用地址，同一个对象

  - ##### 修改任何一个，另外变量看到的也是被修改后的结果。

### 深复制/浅复制：

- ```javascript
  var o={a:1,b:2};
  var o1={};
  for(var prop in o){
  	o1[prop]=o[prop];
  }
  console.log(o1);
  ```

- ```javascript
  var o={a:1,b:2,c:{d:1,e:10}};
  var o1={};
  for(var prop in o){
      o1[prop]=o[prop];
  }
  ```


- ##### 只有深复制才可以真正将一个对象的每个属性都赋值没有引用关系

### 内存泄漏：

#### 大量的不使用的引用对象，没有被标识为null，并且还在不断生成和丢弃。

### 函数：

- #### 函数的结构形态：

  - ```javascript
    function fns(a,b){
        console.log(arguments);
        return;
    }
    var a=fns(4,5);
    ```

  - ###### `function` 定义函数

  - ###### `fns` 函数名 可以自己起，与变量起名方式相同，***函数名也是变量(全局变量)***

  - ###### `(a,b)` 可以向函数中注入的数据，a,b叫做参数，注入几个数据就要声明几个参数

  - ###### `{}` 函数执行的语句块

  - ###### `arguments` 所有通过执行函数时传入的参数列表

  - ###### `return` 跳出当前函数，不再执行`return`以后的语句，并且可以返回一个数据(return 10;)

  - ##### 执行函数

  - ###### `()` 表示执行函数，`fns`是被执行的函数名，执行函数就是将函数中的语句块全部允许

  - ###### `4，5`表示的是传参；

  - ###### 当函数执行完成后，如果使用return返回一个值，将会把返回的值赋值给a

  - ###### 如果函数中没有return，或者直接return没有返回数据，则返回undefined

 - #### 函数创建：

   1. ##### 构造函数法

      - ```javascript
        var fn1=new Function("a","b","s=a+b;return s");
        ```

      - ###### 缺点：

        1. ###### 在调用时，浏览器会将这个函数对象内的所有内容转换为代码，这需要消耗大量资源，所以会慢。

      - ###### 可以由服务端动态生成函数

   2. ##### 命名函数法

      - ```javascript
        function fn1(){
            console.log("aaa");
        }
        ```

        - ##### 可以在当前函数所在script标签的任意位置执行，或者当前函数所在标签后面的所有标签执行。

        - ##### script标签创建时，会把所有命名函数存储到 堆中== >（对象类型），并且栈中以函数名做引用。==>标签开始时就创建出了函数。

        - ##### 标签开始时，定义一个与后面函数相同名字的变量：

          - ```javascript
            console.log(fn1);//fn1函数
            var fn1=3;
            console.log(fn1)//3 ==>原fn1函数会被覆盖
            ```

          - ###### 如果仅定义没有赋值，函数已经定义过了，所有并不覆盖

            - ```javascript
              var fn1;
              console.log(fn1);
              ```

        - ###### 如果定义并且赋值undefined

      - ##### 在ES6中，类中的函数

        - ```javascript
          class Box{play(){}}
          ```

      - ##### 对象中创建函数(方法的创建)

        - ```javascript
          var obj={
          	play(){
          		console.log("aaa");
          	}
          }
          ```

   3. ##### 匿名函数法

      - ```javascript
        var fn1=function(){}
        //匿名函数自执行函数，只能执行一次
        1.(function(){})();
        2.~function(){}();
        3.+function(){}();
        ```

      - ###### 匿名函数 不会在初始化时被定义在栈中，会被运行时定义给变量。

      - ###### 运行到赋值时才将函数赋值给变量，意味着在函数赋值给变量之前不能直接调用。

      - ###### 匿名函数自执行函数，只能执行一次。

- #### 作用域：

  - 全局中定义了一个变量a=5;

  - 在任何位置都可以调用全局变量

  - 函数外使用`var`定义的变量都是全局变量

    - ##### 函数名在外部定义，是全局变量。

  - 在函数内使用 `var`定义的变量都是局部变量，就是该函数的内的局部变量

  - 只能在该函数内调用，不能在函数外或者别的函数中调用

  - 局部变量只能在当前函数内定义，并且函数执行完成后会自动销毁该局部变量。

  - 同名变量，只要在函数内***任何位置***定义看局部变量（使用var 在函数中定义的函数），在这个函数中就不能找到同名全局变量，在局部变量定义之前调用这个局部变量，都是undefined。

    - ```javascript
      var a=5;
      function fn1(){
          if(a>10){
              var a=6;
          }else{
              a=7;
          }
          console.log(a);
      }
      fn1();//7  在没有执行到赋值行前 变量都是undefined，所以进入else语句块
      console.log(a);//5
      ```

    - ```javascript
      var a=5;
      function fn1(){
          if(a==undefined){
              var a=6;
          }else{
              a=7;
          }
          console.log(a);
      }
      fn1();//6==> 在函数内赋值前调用打印都是undefined。
      console.log(a);//5
      ```

  - 局部变量在函数内的优先级高于全局变量，我们可以认为同名变量为变量提升。

  - 全局变量时建立在window中，所以可以通过window调用。

    - ```javascript
      var a=5;
      function fn1(){
          var a=10;
          console.log(a+window.a);
      }
      fn1();
      ```

  - 参数是局部变量。

    - ```javascript
      function fn1(a){
      	if(a===undefined) a=1;
          a++;
          return fn2(a);
      }
      
      function fn2(a){
          a*2;
          return fn3(a);
      }
      
      function fn3(a){
          a+=5;
          return a;
      }
      var s=fn1();
      console.log(s);
      //函数式编程
      ```

- 函数名是全局变量。

  - ```javascript
    var a;
    a(a);
    function a(a){
    	console.log(a);//打印函数自己
        if(a==10){
            var a=6;
        }else{
            a=8;
        }
        console.log(a);//8
    }
    ```


    - ```javascript
      a(5);//在定义与函数名相同变量之前正常
      var a=6;
      //a(5);//放在定义之后，就会报错 ==> 不是一个方法。
      function a(a){
          console.log(a);
          if(a==10){
              var a=6;
          }else{
              a=8;
          }
          console.log(a);
      }
      ```
    
    - ```javascript
      var o={a:1};
      function fn1(obj){
          obj.a=10;
      }
      fn1(o);
      console.log(o);//{a:10}


​      
​      var o={a:1};
​      function fn1(obj){
​          obj={a:10}
​          console.log(obj)//{a:10} ==> 传入的o并没有用到，只给obj赋值
​      }
​      fn1(o);
​      console.log(o);{a:1}
​      ```


  - 少全局，多局部。

    - ```javascript
      var a = {n: 1}
      var b = a
      a.x = a = {n: 2}
      
      console.log(a.n, b.n) //2 1
      console.log(a.x, b.x) //undefined {n:2}
      
      var fn = function () {
      	console.log(fn)//打印函数自己
      }
      fn();
      ```

### json:是一种字符串格式 {}中属性名是用双引号引起来的

1. #### JSON.stringify(对象)：将对象转换为json格式的字符串。

2. #### JSON.parse(json格式的字符串)：将json格式的字符串转换为对象。

3. #### 深复制：

   - 先将对象转换为JSON字符串，字符串是非引用关系

   - 然后再转换为对象，就会自动生成新的对象。

   - ```javascript
     var o=JSON.parse(JSON.stringify(obj));
     obj.a=10;
     console.log(o);
     //无变化
     ```

   - ```javascript
     obj.h=obj.h.toString();
     var str=JSON.stringify(obj);
     var o1=JSON.parse(str);
     console.log(o1);
     ```

4. #### JSON字符串在转换时会丢弃对象中的方法。

