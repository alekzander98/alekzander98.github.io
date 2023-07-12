---
title: NodeJS相关/Web服务器
date: 2021-05-17 19:24:59
categories:
tags: [Server,NodeJS]
---

## Node.js

事件驱动异步I/O单进程的服务端JS环境，具有事件循环。浏览器是JS的前端运行环境；Node JS是JS的后端运行环境，在后端中运行无法调用DOM和BOM等浏览器内置API。

### 模块化

NodeJs基于Commonjs模块化开发的规范

导出：exports/module.exports

- 共同点：导出模块
- 不同点：module.exports它可以导出对象，而exports不可以
- exports = module.exports 引用 本质：原型指向

导入：require(路径)

<!--more-->

##### 内置模块

###### os模块

```js
//导入一个内置的模块
const os = require("os");
console.log(os.hostname);
//操作系统平台
clg(os.platform());
//换行符 windoe \r\n linux/mac \n
// os.EOL 一般用于记录日志
let str = "我是一个" + os.EOL + "标签"
clg(str);
//CPU核数
clg(os.cpus().length);
//总内存大小 字节 => 字节/1024(kb)=>kb/1024(mb)=>mb/1024(G)
clg(os.totalmem()/1024/1024);
```

###### path模块

路径模块

```js
const path = require('path')
let url = '';
//得到url和路径中的文件名称--获取最后一个分隔符后的字符串
clg(path.basename(url));
//得到文件所在目录--获取最后一个分隔符之前的字符串
clg(path.dirname(url));
//得到扩展名 带.的扩展名
clg(path.extname(url));
//拼接路径
//全局变量 __dirname 当前执行脚本所在的路径
const logpath = path.join(__dirname,'data/log.txt')
//const logpath = path.join(__dirname,'data','log.txt')
//const logpath = path.join(__dirname,'../data','log.txt')
//相对地址转换为绝对地址
clg(path.resolve('./data','log.txt'));
//clg(path.resolve('../data','log.txt'));
```

###### url模块

分析url地址

```js
let href = '';
//参数1：分析url地址
//参数2：boolean true/false query属性是否自动转换为对象
//参数3：boolean 一般不用，没有协议时，自动识别域名。
clg(url.parse(href,true));
```

###### JS中内置类URL

get/post方法得到的数据是字符串形式

```js
/*let href = ''
const url = new URL(href);
clg(url.searchParams);
//获取query中相同名称的数据，以数组形式返回
clg(url.searchParams.getAll('id'));
//获取全部的get数据
clg(url.searchParans.entries());
for(let [key,value] of url.searchParans.entries()){
	clg(key,value);
}
*/
let href = '?...'
const query = new URLSearchParams(href)
//search对象
clg(query)
```

###### querystring模块

用于解析和格式化URL查询字符串的实用工具。

###### fs模块

文件交互相关，buffer：容器（缓冲器）一般用来存储数据流。字符串=>二进制=>字符串（文件操作时转码）。

```js
const fs = require('fs');
const path = require('path');
const str = "你好";
//字符串转为buffer流 == 一个中文转为3个字节
//clg(Buffer.from(str));
//创建一个文件
//异步 -- 回调
//回调函数err中的参数，如果返回为null则表示成功，为对象表示写入异常
fs.writeFile(path.resolve('/data/list.txt'),Buffer.from(str),err=>clg(err))
//同步
fs.writeFileSync(path.resolve('/data/list.txt'),str)
//文件异步追加
fs.appendFile(path.resolve('/data/list.txt'),Buffer.from(str),err=>clg(err))

//读取文件
//nodejs中回调函数，第一个参数都是错误
/*fs.readFile(filepath,'utf-8',(err,buffer)=>{
    clg(buffer);
})*/
fs.readFile(filepath,(err,buffer)=>{
    clg(buffer.toString());
})
//删除文件
fs.unlink(newfilepath,err => clg(err));
//查看文件信息
fs.stat()
//判断当前文件或目录是否存在
clg(fs.existsSync(filepath));
//创建目录
const dirpath = path.join(__dirname,'data','aa')
fs.mkdir(dirpath,err => clg(err))
//迭代创建
fs.mkdir(dirpath,{recursive:true},err => clg(err));
//读取目录 返回数组类型
fs.readdir(dirpath,'utf-8',(err,data)=>clg(data));
//删除 默认只删除空目录
fs.rmdir(dirpath,err => clg(err))
//递归删除
fs.rmdir(dirpath,{recursive:true},err => clg(err))
//重命名
```

![1](1.png)

数据流：读流（fs.createReadStream）、写流（fs.createWriteStream）

> fs.createReadStream(filepath).pipe(fs.createWriteStream(newfilepath))

###### 事件触发器（events）

对"发布/订阅"模式的实现，加载events模块后，通过EventEmitter属性建立一个实例化对象。通过`on`方法为事件指定回调函数，最后通过emit方法触发事件。先订阅-后发布

```js
const Event = require('event').EventEmitter
const ev = new Event;
//订阅
ev.on('',(...args)=>clg(args))
//发布
ev.emit('',1,2,3);
ev.emit('',{key:value});
```

##### 第三方模块

首先查看当前项目中有没有package.json文件，--`npm init -y`

npm -S/npm -D:

##### 自定义模块?

自定义模块命名为index.js，如果重命名文档，可以采用修改package.json文档内容，将指向改变。导出变量，函数，类，实例化对象。

```javascript
{
    "main": "index.js"
}
```

##### npm 参数

process.argv:获取的是当前输入的所有命令语句块--数组，通过slice截取数组中有效的元素，`process.argv.slice(2)`。

```js
//node(命令) 执行的文件(.js) [自定义参数]
//获取执行命令中的参数
//1、
let args = {}
let arr = process.argv.slice(2);
arr.forEach((item,index)=>{
    if(item.startsWith('--')){
        args[item.slice(2)] = arr[index + 1];
    }
})

//2、
let args = process.argv.slice(2).reduce((p,c,index,arr)=>{
    if(c.startsWith('--')){
        p[c.slice(2)] = arr[index + 1];
    }
    return p
},{});
//打印结果
console.log(args)
```

##### 环境变量

`node xx | grep xx`

```js
//手动设置环境变量
process.env.NODE_ENV = "dev"
//获取指定的环境变量
if(process.env.NODE_ENV === "dev"){
	console.log('开发中');
}else if(process.env.NODE_ENV = "test"){
    clg("测试中");
}else if(process.env.NODE_ENV = "production"){
    clg("上线")
}else{
    clg('其他')
}
```

###### cross-env插件：

运行跨平台设置和使用环境变量的脚本

安装cross-env插件：`npm i cross-env -D`，在生成的package.json文件中配置。

![image-20210517112543161](2.png)

![image-20210517112657192](3.png)

### web服务器

端口数量：0-65535

成为一个http服务器

- 主机+协议+端口号

```js
//http事件处理
const http = require('http');
//创建一个web服务器
const server = http.createServer()

//侦听事件 浏览器请求 request
//request 请求对象，用于获取浏览器请求 头和体
//response 服务器对浏览器的响应 行 头 体
server.on('request',(request,response)=>{
    //clg("请求了！");
    //当前的请求
    clg(request.uri);
    //获取请求的方法
    clg(request.method);
    //获取请求头--日志
    if(request.url !== '/favicon.ico'){
        clg(new Date().toUTCString());
        clg(request.headers['user-agent']);
    }
    
    //响应头
    response.setHeader('Content-Type','text/html;charset=utf-8');
    //服务器给浏览器响应
    response.end("<h3>你好世界</h3>")
    response.end(JSON.stringify({id:1,name:'张三'}))
})
//监听 端口
//0.0.0.0 => 监听服务器中所有的网址请求
//具体的IP 对应此网卡的IP才可以访问 10 200
//ifconfig => linux
server.listen(3000,'0.0.0.0',()=>{
    clg('服务已启动 http://127.0.0.1:3000');
})
```