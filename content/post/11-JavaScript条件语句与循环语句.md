---
title: JavaScript条件语句与循环语句
date: 2021-03-17 07:51:19
categories:
tags: [JavaScript, Study]
---

1. ##### 条件语句

2. ##### 循环语句

3. ##### 解决基本问题

   <!--more-->

##### 位非运算：以二进制运算，非数值会变成0 

##### `isNaN();` 具有隐式转换功能

##### `Number.isNaN();`不会隐式转换 ，如果传入的不是NaN，或者其他不是数值的字符，返回值都是false。

#### 条件语句：

1. ##### 使用：

   - #### if条件语句

     - ```javascript
       //条件是一个表达式，表达式返回的结果会被隐式转换为布尔值
       if(条件){语句块}
       var a=3;
       if(a=3){}//不管a原先是多少，都会进入语句块
       if(a==3){}
       if(a===3){}
       ```

     - ```javascript
       //前面参与运算，后面判断条件
       var a=2
       if(a++,a==2){console.log("aaa");}
       //语句块只有一句话时，花括号可以省略。
       //如果没有花括号默认只第一句为条件语句块。
       ```

     - ```javascript
       if(){
       	//满足条件时，执行语句块   
       }else{
           //不满足时，执行语句块
       }
       ```

     - ```javascript
       //代码出错时，才会使用try-catch
       try{
           
       }catch(e){
           //只有try语句块的内容报错了，才会执行
       }finally{
           //执行try/catch都会执行
       }throw{
           //异常情况下抛出
       }
       ```

     - ##### if else 与 try catch区别：

       - ##### if else:

         - 条件语句需要预先考虑到可能出现的错误并且提前做条件判断执行。
         - 需要明确的条件
         - 性能消耗小

       - ##### try catch：

         - 不需要预先判断出现错误，由try自身判断。
         - 出错切换
         - 性能消耗大

     - ##### if  else if和多if判断语句：

       1. 多if语句，会进入多次，每个if都进行判断
       2. 多if语句不会对上次的判断进行屏蔽，需要自己将上面判断过的添加进行删减。

     - ##### 多分支条件语句 判断相等 （===）绝对

       - 判断表达式和case中哪一个值相等
         1. break 跳出语句块
         2. default 如果不等同任何一个case的值，则执行default中的语句块

       - **穿越：**一旦一个条件语句块中没有`break`,会自动执行下一个条件的语句块。
       - **状态机**：故意制作穿越，达成游戏技能关联目的。

#### 循环语句：

1. ##### while循环

   - 条件中有多个表达式时，逗号前的表达式在循环前执行，再进行判断==>需要将循环条件放在while()中的最后

     - 深度遍历

       - ```javascript
         var a={value:1,next:{value:2,next:{value:3,next(value:4,next:{value:5,next:null})}}};
         1.
         while(a){
             console.log(a.value);
             a=a.next;
         }
         2.break语句
         while(1){
             if(!a) break;
             console.log(a.value);
             a=a.next;
         }
         ```

2. ##### do while循环

   - 必执行一次，然后根据条件判断是否继续循环

   - 可以规划出起始条件和结束条件，某个变量并不是循环的起始部分。

     - ```bash
       var i=0;
       do{
       	i--;
       	console.log(i);
       	if(i<-10) break;
       }while(i<0);
       ```

3. ##### for循环

   - > for(初始值；条件；向条件外变化的表达式){}
     >
     > - 初始值：开始执行	执行一次
     > - 条件：循环开始判断（进入循环语句块之前）循环几次执行几次（比循环语句块多执行一次）
     > - 向条件外变化的表达式：每次循环语句块结束后执行  循环几次执行几次（和循环语句块执行次数一样）。
     >
     > for(;;){}==>死循环

   - ```javascript
     for(var i=0,s=0;i<=100;s+=i,i++);
     for(var i=0,s=0;s+=i,i++<100;);
     ```

   - 广度优先遍历：

     ```javascript
     var arr=[1,2,3,4,5];
     for(var i=0;i<arr.lenght;i++){
         console.log(arr[i]);
     }
     ```

   - 深度优先遍历：

     ```javascript
     var a={value:1,next:{value:2,next:{value:3,next(value:4,next:{value:5,next:null})}}};
     for(;a;a=a.next) console.log(a.value);
     ```

#### `document.body.innerHTML` 和 `document.write()`都会造成***回流***

- ```bash
  <div id="div1">div1的内容</div>
  <script>
  	var div1 = document.getElementById("div1");
  	document.onclick=function(){
  		document.body.innerHTML+="*";
  		div1.style.color="red";//测试发现点击事件后，div1的内容样式并没有发生变化。
  		
  		var div2=document.getElementById("div1");
  		console.log(div1===div2);//控制台输出false==> div1和div2不一样了
  	}
  </script>
  ```

- 因为第一次页面重构时会自动第一次回流，所以再构造页面时执行

- `document.body.innerHTML`设置值会将所有的内容清空设值

- `document.body.innerHTML+=`会将所有的内容累加其中重新设值

- `document.write()`会直接添加再页面的原有内容的后面

##### 当第一次页面渲染完成后，再执行`document.body.innerHTML`或者`document.write()`，都会再次触发回流，并且表现不同：

- ###### `document.body.innerHTML`设置值会将所有的内容清空设值

- ###### `document.body.innerHTML+=`会将原有的内容累加其中重新设值

- ###### `document.write()`会清空原body中的所有内容，设置为新内容

- ###### 当也买你渲染完成后再次回流以后，造成页面渲染完成前获取的元素将在回流后无法使用，需要重新获取==>因为元素发生了变化。

#### 移动的盒子：

```html
<style>
        #moveBox{
            height: 50px;width: 50px;
            background-color: #000;
            position: relative;
            /* 想要盒子动起来需要添加绝对/相对定位 */
        }
</style>
<div id="moveBox"></div>
    <script>
        var divBox = document.getElementById("moveBox");
        var x=0,y=0,status="right";
        setInterval(() => {
            switch(status){
                case "right":
                    x++;
                    if(x===200) status="down";
                    break;
                case "down":
                    y++;
                    if(y===200) status="left";
                    break;
                case "left":
                    x--;
                    if(x===0) status="up";
                    break;
                case "up":
                    y--;
                    if(y===0) status="right";
                    break;
            }
            divBox.style.left=x+"px";
            divBox.style.top=y+"px";
        }, 16);
    </script>
```

#### 乘法口诀表：

#### 三角形、菱形

#### `break/continue`

- `break`跳出 循环并且不再循环

- `continue`继续 跳出本次循环，继续下次循环，当前本次不执行continue后面的语句。

  - 使用while和do while语句，i++不能写在continue后面。

- `break` 用于死循环跳出

  - ```bash
    var i=0;
    while(true){
    	if(i===5) break;
    	console.log("aaa");
    	i++;
    }
    ```

- `break` 跳出标记，标记后面跟冒号

  - ```javascript
    				var i=0;
    ankang:	while(i<10){
    			var j=0;
    			while(j<10){
    				if(i+j===10) break ankang;
    				console.log(i,j);
    				j++;
    			}
    			i++;
    		}
    		console.log("aaa");
    ```

- `continue`  也可以跳出标记

#### 判断2-100以内的素数：‘

1. ##### for循环：

   - ```javascript
     ak:	for(var i=2,j;i<100;i++){
             for(j=2;j<i;j++){
                 if(i%j===0) continue ak;
             }
         console.log(i+"这是素数");
         }
     ```

2. ##### while循环：

   - ```javascript
     var bool;
     var i=2,j;
     while(i<100){
         j=2;
         bool=true;
         while(j<i){
             if(i%j===0){
                 bool=false;
                 break;
             }
             j++;
         }
         if(bool){
             console.log(i+"是素数");
         }
         i++;
     }
     ```

   - ```javascript
     var j=1,i;
     ak:	while(j++<100){
         i=2;
         while(i<j){
             if(j%i===0) continue ak;
             i++;
         }
         console.log(j+"是素数");
     }
     ```
