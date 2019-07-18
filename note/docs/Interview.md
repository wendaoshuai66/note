#面试题积累1

##Promsie.all实现

记住一下几点

1.Promise.all 的返回值是一个 promise 实例

如果传入的参数为空的可迭代对象，Promise.all 会 同步 返回一个已完成状态的 promise

如果传入的参数中不包含任何 promise,Promise.all 会 异步 返回一个已完成状态的 promise (不包含 promise，意味着不会一直 pending 和 rejected )

其它情况下，Promise.all 返回一个 处理中（pending） 状态的 promise.

2.Promise.all 返回的 promise 的状态

如果传入的参数中的 promise 都变成完成状态，Promise.all 返回的 promise 异步地变为完成。

如果传入的参数中，有一个 promise 失败，Promise.all 异步地将失败的那个结果给失败状态的回调函数，而不管其它 promise 是否完成

在任何情况下，Promise.all 返回的 promise 的完成状态的结果都是一个数组


```
Promise.all = function(prosmises) {
                if (!Array.isArray(prosmises)) {
                    throw new TypeError('You must pass to an array')
                }
                return new Promise(function(resolve, reject) {
                    var len = prosmises.length,
                        results = [],
                        count = len;
                    if (len == 0) {
                        resolve()
                    }

                    function resolves(inxdex) {
                        return function(value) {
                            resolveAll(inxdex, value)
                        }
                    }

                    function resolveAll(index, value) {
                        results[index] = value;
                        --count;
                        if (count == 0) {
                            resolve(results)
                        }
                    }

                    function rejects(value) {
                        reject(value)
                        return
                    }
                    for (let i = 0; i < len; i++) {
                        Promise.resolve(prosmises[i]).then(resolves(i), rejects)
                    }
                })
            }```

##Promsie.race实现

Promise.race 功能

Promise.race(iterable) 返回一个 promise，一旦 iterable 中的一个 promise 状态是 fulfilled / rejected ，那么 Promise.race 返回的 promise 状态是 fulfilled / rejected

```
let p = Promise.race([p1, p2, p3]);
```

只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给 p 的回调函数。

Promise.race 的特点

Promise.race 的返回值是一个 promise 实例

1.如果传入的参数为空的可迭代对象，那么 Promise.race 返回的 promise 永远是 pending 态
如果传入的参数中不包含任何 promise，Promise.race 会返回一个处理中（pending）的 promise
如果 iterable 包含一个或多个非 promise 值或已经解决的promise，则 Promise.race 将解析为 iterable 中找到的第一个值。


```
Promise.race = function(promsies) {
            if (!Array.isArray(promsies)) {
                throw new TypeError('You must pass to an array')
            }
            return new Promise(function(resolve, reject) {
                var len = promsies.length,
                    result = [],
                    count = len;

                if (len === 0) {
                    //空的可迭代对象;
                    //用于在pending态
                } else {
                    function resolver(value) {
                        resolve(value)
                    }

                    function rejects(value) {
                        reject(value)
                        return
                    }
                    for (var i = 0; i < len; i++) {
                        Promise.resolve(promsies[i]).then(resolver, rejects)
                    }
                }
            })
        }
```


##为啥 await 在 forEach 中不生效 


###不知道你有没有写过类似的代码，反正以前我是写过


```
function test() {
	let arr = [3, 2, 1]
	arr.forEach(async item => {
		const res = await fetch(item)
		console.log(res)
	})
	console.log('end')
}

function fetch(x) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(x)
		}, 500 * x)
	})
}

test()
```

我当时期望的打印顺序是

```
3
2
1
end
```

结果现实与我开了个玩笑，打印顺序居然是

```
end
1
2
3
```

###为什么

其实原因很简单，那就是 forEach 只支持同步代码。

我们可以参考下 Polyfill 版本的 forEach，简化以后类似就是这样的伪代码

```
while (index < arr.length) {
		// 也就是我们传入的回调函数
		callback(item, index)
}
```

从上述代码中我们可以发现，forEach 只是简单的执行了下回调函数而已，并不会去处理异步的情况。并且你在 callback 中即使使用 break 也并不能结束遍历

###怎么解决？

一般来说解决的办法有两种。

第一种是使用 Promise.all 的方式

```

async function test() {
	let arr = [3, 2, 1]
	await Promise.all(
		arr.map(async item => {
			const res = await fetch(item)
			console.log(res)
		})
	)
	console.log('end')
}

```

这样可以生效的原因是 async 函数肯定会返回一个 Promise 对象，调用 map 以后返回值就是一个存放了 Promise 的数组了，这样我们把数组传入 Promise.all 中就可以解决问题了。但是这种方式其实并不能达成我们要的效果，如果你希望内部的 fetch 是顺序完成的，可以选择第二种方式

另一种方法是使用 for...of

```
async function test() {
	let arr = [3, 2, 1]
	for (const item of arr) {
		const res = await fetch(item)
		console.log(res)
	}
	console.log('end')
}

```

因为 for...of 内部处理的机制和 forEach 不同，forEach 是直接调用回调函数，for...of 是通过迭代器的方式去遍历

```
async function test() {
	let arr = [3, 2, 1]
	const iterator = arr[Symbol.iterator]()
	let res = iterator.next()
	while (!res.done) {
		const value = res.value
		const res1 = await fetch(value)
		console.log(res1)
		res = iterator.next()
	}
	console.log('end')
}
```

以上代码等价于 for...of，可以看成 for...of 是以上代码的语法糖。

###延伸

```
let a = 0;
        let test = async() => {
            a = a + await 10;
            console.log(a)
        }
        test()
        console.log(++a)
```

一般会认为输出

```
1
11
```

实际输出

```
1
10
```

这里await会对a 冻结