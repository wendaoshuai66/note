#NodeJS入门

##什么是Node

NodeJS本质上是一个JavaScript的解释器，是JavaScript的运行环境。它是一个后端服务器程序，并且允许使用JavaScript语言定义数据结构，可以操作磁盘文件和搭建http服务。

#####注意：NodeJS并不是一个Web服务器，它是可以搭建Web服务器的，可以说Web服务器是Nodejs的一个产物。

##为什么要用NodeJS
NodeJS有以下特性值得我们去用 

1.可以提供高性能的Web服务(Node作者的初衷)

2.IO性能强大(发送数据和接收数据，所依赖的条件就是输入和输出)

3.事件处理机制完善

4.天然能够处理DOM(JavaScript语言的能力)

5.社区非常活跃，生态圈日趋完善

##NodeJS的优势在哪里

1.处理大流量数据

2.适合实时交互的应用(在线聊天系统)

3.完美支持对象型数据库(Mongodb)

4.异步处理并发连接(很多人同事链接一台服务器就叫做并发连接)

##学习NodeJS的前置知识

1.JavaScript

2.ES6

3.一些服务器相关的知识(网络相关的知识)

4.最好在Linux系统上进行开发

##相关资料和学习网站

1.[官方网站](https://nodejs.org/en/)


2.[中文网站](https://nodejs.org/zh-cn/)

##安装NodeJS

在上面官网下载。不同的操作系统下载相应的版本，学习的话最好安装最新版(LTS:表示长期稳定版本)。Source Code就是NodeJS的源代码，是用C++编写的。

切换node 版本 可以参考该[网站](https://www.cnblogs.com/kongxianghai/p/5660101.html)

##检查是否安装成功

```
node -v
```

上面命令是检查Node是否安装成功，成功的话返回安装Node的版本。

##包管理器npm

npm是依附于node的包管理器，在下载node的时候会相应把npm下载下来。检查npm是否下载使用以下命令,成功的话返回npm的版本号

```
npm -v
```


###显示npm子命令列表

```
npm -help
```

###升级npm

```
npm install npm -g
```

###查找包

```
npm search xxx
```
###npm所能做的事

允许用户从npm服务器下载别人编写的第三方包到本地使用。

允许用户从npm服务器下载并安装别人编写的命令行程序到本地使用。

允许用户将自己编写的包或命令行程序上传到npm服务器供别人使用

###npm的官网


[npm的官网](https://www.npmjs.com/)



##搭建一个简单的服务

```
//从node原声API中引入http模块
const http = require("http");
//使用http的createServer创建一个服务
http.createServer((req,res) => {
    //req是服务器接收浏览器请求对象。
    //res是服务器响应浏览器请求对象。

    //定义HTTP响应头
        //第一个值是响应的状态值。
        //第二个值是以json格式返回的响应头的必要信息。
    res.writeHead(200,{
        'Content-Type' : 'text/plan'
    });

    //发送响应数据
    res.end('Hello World! \n');

//使用级联函数对监听端口的设定
}).listen(8080)
//服务运行后输出一行信息
console.log('server is running...');

//开启服务
    //在终端中使用 node node2.js
        //输出 server is running...
//在浏览器访问
    //如果node在本机上：127.0.0.1:8080
    //如果在远程服务器上：服务器ip:8080

```

##NodeJS的REPL环境


###什么是REPL

REPL是一种交互解释器，在终端中，直接敲node然后回车，就进入到了nodeREPL环境


###REPL能干什么？

node的REPL环境可以做一些简单的运算，和做一些简单的代码验证。在这个环境下可以正常的书写代码，相当于浏览器控制台的console。

###Node REPL中的命令

![Node REPL中的命令](https://wendaoshuai66.github.io/study/note/images/repl.png)

##Node.js回调机制及事件驱动模型


###什么是回调

函数的调用分为三种：同步调用、回调和异步调用。

回调是一种双向调用

#####被调用的一个函数在被调用的使用会反过来调用它的主调函数，这种情况叫做双向调用

JavaScript通过回调函数来实现回调。

##阻塞与非阻塞

在NodeJS入门就说到Node是非阻塞是IO，什么是阻塞什么是非阻塞呢？

1.阻塞和非阻塞关注的是程序在等待调用结果(消息，返回值)时的状态

2.阻塞就是做不完不准回来。

3.非阻塞就是当函数执行的时候，把函数的执行权先交给外界环境，等待其中IO完成之后，再把执行权返回给该函数。(通俗的将：非阻塞就是你先做，我先看看有其他事没有，完了告诉我一声。)

##Nodejs的回调函数

阻塞式的代码

```
//引入node中的文件系统模块
const fs = require('fs');
//同步读取,读取的信息按照16进制保存的
let data = fs.readFileSync('date.txt');      
console.log(data.toString());

```

非阻塞的代码

```
const fs = require('fs');
//异步读取
    //第一个参数是读取文件的文件路径
    //第二个参数是异步读取文件的回调函数
fs.readFile('test.js',function(err,data){
    //回调函数的第一个参数是err
    //第二个参数是读取的信息。
    if(err){
        return console.log(err);
    }
    console.log(data.toString());
})
console.log('程序执行完毕');
```

##NodeJS的事件驱动模型

![NodeJS的事件驱动模型](https://wendaoshuai66.github.io/study/note/images/eventloop.png)

nodejs是一个单进程单线程的运行程序，并不能并发的去完成更多的事情，只能通过事件回调实现这种并发的效果。nodejs中的每一个API都是异步执行的，而且都是作为一个独立的线程在运行，使用异步函数调用就可以使用这种机制来并发处理。

###事件处理流程

每当一个事件注册了之后就会把这个事件放入事件队列中，然后通过事件轮询依次触发事件。

###事件与事件绑定

NodeJS通过引入events模块，来进行事件的绑定流程如下：

1、引入events模块

```
const events = require('events');
```

2、创建eventEmitter对象

```
const eventEmitter = new events.EventEmitter();
```
3、绑定事件处理程序

```
//事件处理函数
const connectHandle = () => {
    console.log('connectHandle');
}   
//事件绑定
    //使用eventEmitter.on方法绑定事件
        //第一个参数：事件名称
        //第二个参数：事件处理函数
eventEmitter.on('connection',connectHandle);   
```

4、触发事件

```
eventEmitter.emit('connection');
```

###EventEmitter类自身的方法

####EventEmitter.listenerCount(obj,event)

可用来获取某个对象的指定事件的事件处理函数的数量。两个参数：

1.第一个参数是用于指定目标是哪个对象


2.二个参数是指定哪个事件

```
EventEmitter.listenerCount(http,'request');

```

###EventEmitter类自身的事件

####newListener事件
当对继承了EventEmitter类的子类实例对象绑定事件处理函数 的时候，都会触发EventListener类的newListener事件。

在事件处理函数中，可使用两个参数，其中第一个参数为被绑定的事件名，第二个参数为被绑定的事件处理函数。

```
eventEmitter.on('newListener',(eventName,handler)=>{
	console.log(eventName);
})
eventEmitter.on('request',()=>{
	console.log('listener');
})
```

####removeListener事件

当对继承了EventEmitter类的子类实例对象取消事件处理函数时，都将触发EventEmitter类的removeListener事件。

在事件处理函数中，可使用两个参数，其中第一个参数为被取消事件处理函数的事件名，第二个参数为被取消的事件处理函数。

```
eventEmitter.on('removeListener',(eventName,handler)=>{
		console.log(eventName);
})
eventEmitter.removeAllListeners('request');
```

##Node.js 模块化

###模块化的概念与意义

1.为了让Nodejs的文件可以相互调用，Nodejs提供了一个简单的模块系统

2.模块是Nodejs应用程序的基本组成部分

3.文件和模块是一一对应的。一个Nodejs文件就是一个模块

4.这个文件可能是JavaScript代码、JSON或者编译过的C/C++扩展

5.Nodejs中存在4类模块(原生模块和3种文件模块：3种文件模块是第三方提供的，原生模块是node原生支持的)

###Nodejs中的模块

NodeJS通过require方法加载模块，通过exports导出模块

```
module.exports导出单个模块
```

```
exports.xxx=xxx 导出模块对象
```

###Node.js 的模块加载流程


![Node.js 的模块加载方式](https://wendaoshuai66.github.io/study/note/images/require.png)

#####文件模块缓存区的作用：防止模块被反复的加载。节省内存，加快了模块的加载速度

1.开始require加载模块，首先判断是否在文件模块缓存区中，如果在就在缓存区中通过exports导出;

2.如果不在文件模块缓存区就去查找是否是原生模块(原生模块和第三方模块是不在同一个内存缓存区的),
跳转查看是否在原生模块缓存区中，如果在的话就在缓存区中通过exports导出;

如果不在原生模块缓存区中，那么就去加载原生模块，加载原生模块的同时，把加载的原生模块放到原生模块缓冲区中，通过exports导出;

3.如果不是原生模块，就是查找第三方的文件模块，根据扩展名载入响应的文件模块，把载入的文件模块放入文件模块缓存区中，通过exports导出。


###require方法接受一下几种参数的传递

http、fs、path等原生模块

./mod或者../mod等相对路径的文件模块

/pathtomodule/mod 绝对路径的文件模块

mod 非原生模块的文件模块

###模块的三种加载方式

1.从文件模块缓存区中加载

2.从原生模块加载

3.从文件中加载

###模块加载代码示例

test.js为子模块

```
const Test = function(){
    var name;
    this.setName = function(newName){
        name = newName;
    }
    this.sayName = function(){
        console.log(name);
    }
}
modules.exports = Test;
```
main.js为主模块

```
const Test = require('./test');
const test = new Test();
test.setName('wang');
test.sayName(); //wang
```

##Node.js函数

在js中，函数可以当做另外一个函数参数

废话不多说，直接上代码

```
function say(word){
    console.log(word)
}
function execute(somefuction,value){
    somefuction(value)
}
execute(say,'hello ,word')

```
##Node.js 路由

###什么是路由

简单的说，把一个总路线分成多个方向，通过调用不同方向的标签，执行不同的程序。

```
路由表示 controller/action
```

一个浏览器网址的ur可能是这样的

```
www.ctomorrow.top/about/phone
```

这其中的/about/phone就构成了一个路由，about是相应的控制器，action是对应的控制ID。

###路由

当我们在浏览器地址栏输入地址访问页面的时候，一个完整的地址可能是这样的：协议://域名:端口:路由?路由参数

###我们先来实现一个路由

创建一个http模块用于创建http服务

```
const http = require("http");
const url = require('url'); //引入url模块进行url解析

function start(router){
    const onRequest =(req,res) => {
        //获取路由地址
        let pathname = url.parse(req.url).pathname;
        res.writeHead(200,{
            'Content-Type' : 'text/plain;charset=utf8',
        })
    }
    http.createServer(onRequest).listen(8080);
}
exports.start =start;
```

创建主模块中调用http模块

```

const http = require('./http');
http.start();
```

创建route模块来进行不同路由的管理,不同的路由输出不同的信息。

```
function route(pathname,res){
    console.log(pathname);
    if(pathname == '/'){
        res.write('欢迎来到首页');
    }else if(pathname == '/a/g'){
        res.write('欢迎来到/a/g');
    }else {
        res.write(`您访问的${pathname}不存在！`);
    }
    res.end();
    
}

exports.route = route;
```

在主模块中调用路由，并把路由传入http模块调用

```
const http = require('./http');
const router = require('./route');
http.start(router.route);
```

在http模块中，把路由地址传入路由模块。


```
const http = require("http");
const url = require('url');

function start(router){
    const onRequest =(req,res) => {
        let pathname = url.parse(req.url).pathname;
        res.writeHead(200,{
            'Content-Type' : 'text/plain;charset=utf8',
        })
        //加上这一句
        router(pathname,res)
    }
    http.createServer(onRequest).listen(8080);
}
exports.start =start;
```

###请求参数解析

####GET请求

由于GET请求直接被嵌入在路径中，URL是完整的请求路径，包括了?后面的部分，因此可以手动解析后面的内容作为GET请求的参数。

通过使用ur和util模块来输出

```
const url = require('url');
const util = require('util');
const http = requier('http');

http.createServer((req,res)=>{
    //util的inspec方法是格式化输出
    //url.parse是对url进行解析
    console.log(util.inspect(url.parse(req.url,true)));
}).listen(8080);
```

```
Url {
  protocol: null,
  slashes: null,
  auth: null,
  host: null,
  port: null,
  hostname: null,
  hash: null,
  search: '',
  query: {},
  pathname: '/favicon.ico',
  path: '/favicon.ico',
  href: '/favicon.ico' }
```

####POST请求
POST请求的内容全部的都在请求体中，http.ServerRequest并没有一个属性内容为请求体，原因是等待请求体传输可能是一件耗时的工作。比如上传文件，而很多时候我们可能不需要理会请求体中的内容，恶意的POST请求会大大耗费服务器的资源，所有nodejs默认是不会解析请求体的。只有当需要的时候，手动来做。

通过querystring模块来解析POST请求

```
const http = require('http');
const querystring = require('querystring');
const util = require('util');
http.createServer((req,res) => {
    var post = '';      //用于暂存请求体的内容
    req.on('data',(chunk) => {
        //通过req的data事件监听函数，每当接受到请求体的数据，就累加到变量中
        post += chunk;
    })
    req.on('end',() => {
        //在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回。
        post = querystring.parse(post);
        res.end(util.inspect(post));
    })
```
##Node.js全局方法和工具函数

###全局对象global

JavaScript 中有一个特殊的对象，称为全局对象（Global Object），它及其所有属性都可以在程序的任何地方访问，即全局变量。

在浏览器 JavaScript 中，通常 window 是全局对象， 而 Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global 对象的属性。

#####永远使用声明变量以避免引入全局变量，因为全局变量会污染 命令空间，提高代码的耦合风险。

###__filename

__filename表示当前正在执行的脚本的文件名，它将输出文件所在位置的绝对路径，且和命令行参数所指的文件名不一定相同.如果在模块中，返回的值是模块的路径。
```
console.log(__filename)
```

###__dirname

__dirname 表示当前执行脚本所在的目录。

```
// 输出全局变量 __dirname 的值
console.log( __dirname );
```

###setTimeout(cb,ms)

和浏览器中执行的js代码的setTimeout相同，在指定的毫秒后执行指定函数cb，setTimeout()只执行一次指定函数。返回一个定时器对象。

```
const timer = setTimeout(()=> {
    console.log('time');
},2000)
console.log(timer);
```

###clearTimeout()

clearTimeout(e) 全局函数用于停止一个之前通过setTimeout创建的定时器，参数t是通过setTimeoit函数创建的定时器对象。

```
const timer = setTimeout(()=> {
    console.log('timer');
},2000)
clearTimeout(timer);
```


###setInterval(cb,ms)

setInterval(cb,ms)全局函数在指定的毫秒数后执行指定函数(cb),返回一个定时器对象，可以使用clearInterval函数清楚定时器，setInterval方法会不停的调用函数，直到clearInterval被调用或窗口被关闭。

```
setInterval(()=>{
    console.log('hello');
},1000);
```
###console方法


![console方法](https://wendaoshuai66.github.io/study/note/images/console.png)

###process

process 是一个全局变量，即 global 对象的属性。

它用于描述当前Node.js 进程状态的对象，提供了一个与操作系统的简单接口。通常在你写本地命令行程序的时候，少不了要 和它打交道。

###常用工具


util是一个Nodejs核心模块，提供常用函数的集合，用于弥补核心JavaScript的功能，过于精简的不足。

####引入util

```
const util = require('util');
```

####util.inherits

util.inherits(constructor,superContructor)是一个实现对象间原型继承的函数。

```
function Car(){
    this.name = 'wang';
}
function Curze(){
    this.type = 'curze';
}
util.inherits(Curze,Car);
const curze = new Curze();
console.log(curze.__proto__.__proto__);     //Car
```

####util.inspect


util.inspect(object,[showHidden],[depth],[colors])是一个将任意对象转化程字符串的方法，通常用于调试和错误输出。它至少接收一个参数object，即要转换的对象。

showHidden是一个可选参数，如果值为true，将会输出更多的隐藏信息。
 
depth表示最大递归层数，如果对象很复杂，你可以指定层数以控制输出信息的多少。如果不指定depth，   默认会递归两层，指定为null表示不限递归赠书完整遍历对象。
 
如果color值为true，输出格式将会一ANSI颜色编码，通常用于在终端显示更漂亮的效果。
 
 
#####特别要指出的是，util.inspect并不会简单的直接把对象转换为字符串，即使对象定义了toString方法也不会调用。

####util.isArray(object)

如果给定的参数object是一个数组返回true，否则返回false

```
console.log(util.isArray([]));      //true
```

####util.isRegExp(object)

如果给定的参数object是一个正则表达式返回true，否则返回false

```
console.log(util.isRegExp(/[]/g));  //true
```

####util.isDate(object)

如果给定的参数object是一个日期返回true，否则返回false

```
console.log(util.isDate(new Date()));   //true
```

####util.isError(object)

如果给定的参数object是一个错误对象返回true，否则返回false

```
console.log(util.isError(new Error()));

```

####一个更强大的功能库–underscore.js

##Node.js文件系统

###文件模块

引入文件模块

```
const fs = require(‘fs’);
```

###异步与同步文件操作的区别

异步方法性能更高、速度更快、而且没有阻塞

###打开文件–异步


```
fs.open(path,flags,mode,callback);
```
1.path：文件的路径

2.flags：文件打开的行为。

3.model：设置文件模式(权限),文件创建默认权限为0666(可读可写);

r：以读取模式打开文件。如果文件不存在抛出异常

r+：以读写模式打开文件，如果文件不存在抛出异常

rs：以同步的方式读取文件

rs+：以同步的方式读取和写入文件

w：以写入模式打开文件，如果文件不存在则创建

wx：类似于w，但是如果文件路径存在，则文件写入失败

w+：以读写模式打开文件，如果文件不存在则创建

wx+：类似w+，但是如果文件路径存在，则文件读写失败

a：以追加模式打开文件，如果文件不存在则创建

ax：类似于a，但是如果文件路径存在，则文件追加失败

a+：以读取追加模式打开文件，如果文件不存在则创建

ax+：类似a+，但是如果文件路径存在，则文件读取追加失败

4.callback：回调函数，带有两个参数：callback(err,fd)

err是错误对象

fd：表示所打开的文件有多少行

```
fs.open('test.js','r+',(err,fd) => {
    if(err){
        return console.log(err);
    }
    console.log(fd);
});
```

###获取文件信息

```
fs.stat(path,callback)
```

path：文件路径
callback：回调函数，带有两个参数：err，states，states是fs.States对象

```
fs.stat('test.js',(err,stats)=> {
    if(err){
        return console.error(err);
    }
    console.log(stats);
})
```

stats类中的方法



![stats类中的方法](https://wendaoshuai66.github.io/study/note/images/fsstate.png)

###写入文件

####同步写入文件

```
fs.writeFileSync('test1.js','fdsafdaf');
console.log('写入成功');
```

####异步写入文件

```
fs.writeFile(file,data,{options},callback)
```


writeFile直接打开文件默认是w模式，所以如果文件存在，该方法写入的内容会覆盖旧的文件内容

file：文件名或文件描述符

data：要写入文件的数据，可能是String(字符串)或Buffer(缓冲)对象;

options：该参数是一个对象，包含{encoding、mode、flag}。默认编码utf8，模式为0666，flag为‘w’

callback：回调函数，回调函数只包含出错信息参数(err),在写入失败时返回。

```
console.log('准备写入文件!');
fs.writeFile('test1.js','写入成功!!!!!',(err)=> {
    if(err){
        return console.error(err);
    }
    console.log('数据写入成功!');
    console.log('-------我是分割线--------');
    console.log('读取写入的数据');
    fs.readFile('test1.js',(err,data) => {
        if(err){
            return console.error(err);
        }
        console.log('异步读取文件数据:' + data.toString());
    })
})

```

###读取文件

####同步读取文件

```
const data = fs.readFileSync('test1.js');
console.log(data.toString());
```

####异步读取文件

使用fs.readFile(path,{options},callback)

path:读取文件的路径

options:{encoding,flag},encoding:null,flag:r

callback:err,data;第一个参数是err,第二个参数是读取出的信息。

```

fs.readFile('test1.js',{encoding:'utf8'},(err,data) => {
    if(err){
        return console.error(err);
    }
    console.log(data);
})
```

使用fs.read(fd,buffer,offset,length,position,callback);

fd：通过fs.open()方法返回的文件描述符

buffer：数据写入的缓冲区

offset：缓冲区写入的写入偏移量

length：要从文件中读取的字节数

position：文件读取的起始位置，如果position的值为null，则会从当前文件指针的位置读取

callback：回调函数，有三个参数err、byteRead、buffer。err为错误信息，byteRead表示读取的字节数，buffer为缓冲区对象

```
var buffer = new Buffer.alloc(1024);
fs.open('test1.js','r+',(err,fd) => {
    if(err){
        return console.error(err);
    }
    console.log('打开文件成功！');
    console.log('准备打开文件！');
    fs.read(fd,buffer,'0',buffer.length,0,(err,byteRead,buffer) => {
        if(err){
            return console.error(err);
        }
        console.log(byteRead + '字符被读取');
        console.log(buffer.toString());
    })
})
```

###关闭文件

fs.close(fd,callback) 异步关闭文件

fd：通过fs.open()方法返回的文件描述符

callback：回调函数

```
fs.open('test1.js','r+',(err,fd) => {
    if(err){
        return console.error(err);
    }
    console.log('文件打开成功！');
    fs.close(fd,()=>{
        console.log('文件关闭成功！');
    })
}) 
```

fs.closeSync(fd) 同步关闭文件

fd：通过fs.open()方法返回的文件描述符

```
fs.open('test1.js','r+' ,(err,fd) => {
    if(err){
        return console.error(err);
    }
    console.log('文件打开成功！');
    fs.closeSync(fd);
    console.log('文件关闭成功！');
})
```

###截取文件

fs.ftruncate(fd,len,callback) 异步截取

fd：通过fs.open()方法返回的文件描述符

len：文件内容截取的长度

callback：回调函数

```
fs.open('test1.js','r+',(err,fd) => {
    if(err){
        return console.error(err);
    }
    console.log('打开文件成功！');
    console.log('开始截取文件');
    fs.ftruncate(fd,10,() => {
        console.log('截取文件成功！');
        fs.close(fd,() =>{
            console.log('文件关闭成功！');
        })
    });
})
```

###删除文件

fs.unlink(path,callback);

###创建目录

fs.mkdir(path,[options],callback)

###读取目录

fs.readdir(path,callback)

###删除目录

fs.rmdir(path,callback)








