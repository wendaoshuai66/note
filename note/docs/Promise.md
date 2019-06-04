#Promise
##什么是Promise

```
function add(getX,getY,cb){
	var x,y;
	getX(function(val){
	   x= val;
	   // 两者都准备好了？
		if(y!=undefined){
		  cb(x+y) // 发送加法的结果
		}
	})
	getY(function(val){
		y= val;
		// 两者都准备好了？
		if(x!=undefined){
		 cb(x+y) // 发送加法的结果
		}
	})
}
var fetchX =function(fn){
	setTimeout(function(){
	 fn(1)
	},1000)
}
var fetchY =function(fn){
	setTimeout(function(){
	 fn(3)
	},2000)
}
add(fetchX,fetchY,function(sum){
  console.log(sum)//4
})
```

花点儿时间来感受一下这段代码的美妙（或者丑陋），我耐心地等你。

虽然丑陋是无法否认的，但是关于这种异步模式有一些非常重要的事情需要注意。

在这段代码中，我们将x和y作为未来的值对待，我们将add(..)操作表达为：（从外部看来）它并不关心x或y或它们两者现在是否可用。换句话所，它泛化了 现在 和 稍后，如此我们可以信赖add(..)操作的一个可预测的结果。

通过使用一个临时一致的add(..)——它跨越 现在 和 稍后 的行为是相同的——异步代码的推理变得容易的多了。


更直白地说：为了一致地处理 现在 和 稍后，我们将它们都作为 稍后：所有的操作都变成异步的。

当然，这种粗略的基于回调的方法留下了许多提升的空间。为了理解在不用关心 未来的值 在时间上什么时候变得可用的情况下推理它而带来的好处，这仅仅是迈出的一小步。


##Promise值

我们先简单地看一下我们如何通过Promise来表达x + y的例子：

```
function add (xPromise,yPromise){
         // `Promise.all([ .. ])`接收一个Promise的数组，
        // 并返回一个等待它们全部完成的新Promise
        return Promise.all([xPromise,yPromise])
         // 当这个Promise被解析后，我们拿起收到的`X`和`Y`的值，并把它们相加
        .then(values=>{
         // `values`是一个从先前被解析的Promise那里收到的消息数组
            return values[0]+values[1]
        })
    }

var fetchX =function(){
   return new Promise((reslove,reject)=>{
        setTimeout(function(){
            reslove(1)
        },1000)
   })
}
var fetchY =function(){
  return new Promise((reslove,reject)=>{
        setTimeout(function(){
            reslove(2)
        },2000)
   })
}

// 为了将两个数字相加，我们得到一个Promise。
// 现在我们链式地调用`then(..)`来等待返回的Promise被解析
add(fetchX(),fetchY()).then(sum=>{
 console.log(sum)//3
})
```

注意： 在add(..)内部。Promise.all([ .. ])调用创建了一个promise（它在等待promiseX和promiseY被解析）。链式调用.then(..)创建了另一个promise，它的return values[0] + values[1]这一行会被立即解析（使用加法的结果）。这样，我们链接在add(..)调用末尾的then(..)调用——在代码段最后——实际上是在第二个被返回的promise上进行操作，而非被Promise.all([ .. ])创建的第一个promise。另外，虽然我们没有在这第二个then(..)的末尾链接任何操作，它也已经创建了另一个promise，我们可以选择监听/使用它。
