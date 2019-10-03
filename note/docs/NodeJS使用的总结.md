#NodeJS使用的总结

##首先提出一个问题，大家在开发Node.js的时候都知道异步的嵌套非常麻烦，有人叫回调地狱也有人叫回调黑洞，那么问题来了，如何解决改问题：

#####回答该问题之前可以先说说什么是异步编程

在我们开发过程中经常会遇到如何表达和控制持续一段时间的程序行为。比如在等待用户输入，从数据库或文件中请求数据，通过网络发送数据并等待响应，或者是固定时间间隔内重复执行任务（比如动画）等，在诸如此类 的场景中，程序都需要管理这段时间间隙的状态。事实上，程序中现在运行的部分和将来运行的部分之间的关系就是异步编程的核心。

#####再说说什么是事件循环

JavaScript 引擎并不是独立运行的，它运行在宿主环境中，现在宿主环境大部分Web浏览器，Node.js 这样的工具进入服务器领域，从机器人到电灯泡。

这些环境都有一个共同“点”，即它们都提供了一种机制来处理程序中多个块的执行，且执行每块时调用JavaScript 引擎，这种机制被称为事件循环。换句话说，JavaScript 引擎本身并没有时间的概念，只是一个按需执行 JavaScript 任意代码 片段的环境。“事件”(JavaScript 代码执行)调度总是由包含它的环境进行。

setTimeout(..) 并没有把你的回调函数挂在事件循环队列中。它所做的是设 定一个定时器。当定时器到时后，环境会把你的回调函数放在事件循环中，这样，在未来 某个时刻的 tick 会摘下并执行这个回调。



1⃣️.Express 我们可以 Promise / Defferred采用模式的诸多类库，如Q.js Step wind 


2⃣️.ES6 原生的Generator 函数 + Promise 对象 KOA1时代的代表

3⃣️.Async await 函数 KOA2 推崇 的办法代码如下：

```
var fetch = require('node-fetch'); 
var fetchData = async function () { 
var r1 = await fetch('https://api.github.com/users/github'); 
var json1 = await r1.json(); console.log(json1.bio); }; 
fetchData();
```