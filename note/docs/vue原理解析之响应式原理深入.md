#vue 原理解析之响应式原理深入

首先来看图

![](https://wendaoshuai66.github.io/study/note/images/vue6.png)

![](https://wendaoshuai66.github.io/study/note/images/vue7.png)

##响应式对象

###Object.defineProperty

Object.defineProperty 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象，先来看一下它的语法：

```
Object.defineProperty(obj, prop, descriptor)
```

具体的可以去参阅它的[文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)。这里我们最关心的是 get 和 set，get 是一个给属性提供的 getter 方法，当我们访问了该属性的时候会触发 getter 方法；set 是一个给属性提供的 setter 方法，当我们对该属性做修改的时候会触发 setter 方法。

一旦对象拥有了 getter 和 setter，我们可以简单地把这个对象称为响应式对象。那么 Vue.js 把哪些对象变成了响应式对象了呢，接下来我们从源码层面分析。

#initState
