#探索Redux原理

##前言

react和状态管理redux是紧密结合的，而本身又没有任何联系。react可以不使用redux管理状态，redux也可以脱离react独立存在。随着react的项目越来越复杂，state变的繁重，各种prop和state的转变让我们在开发过程中变得头晕眼花，react本来就是一个专注于UI层的库，本不应该让繁杂的prop和state的逻辑掺杂进来。于是Flux的架构出现了，Flux架构模式用于抽离react的state能更好的去构建项目，Flux架构模式的实践有好多中，显然redux是成功的。

###redux的设计原则

redux有三大设计原则

1.单一数据源

2.状态是只读的

3.使用纯函数编写reducer

####单一数据源

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

这让同构应用开发变得非常容易。来自服务端的 state 可以在无需编写更多代码的情况下被序列化并注入到客户端中。由于是单一的 state tree ，调试也变得非常容易。在开发中，你可以把应用的 state 保存在本地，从而加快开发速度。此外，受益于单一的 state tree ，以前难以实现的如“撤销/重做”这类功能也变得轻而易举。

####State 是只读的

唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

这样确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化处理，且严格按照一个接一个的顺序执行，因此不用担心竞态条件（race condition）的出现。 Action 就是普通对象而已，因此它们可以被日志打印、序列化、储存、后期调试或测试时回放出来。

```
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

####使用纯函数来执行修改

为了描述 action 如何改变 state tree ，你需要编写 reducers。

Reducer 只是一些纯函数，它接收先前的 state 和 action，并返回新的 state。刚开始你可以只有一个 reducer，随着应用变大，你可以把它拆成多个小的 reducers，分别独立地操作 state tree 的不同部分，因为 reducer 只是函数，你可以控制它们被调用的顺序，传入附加数据，甚至编写可复用的 reducer 来处理一些通用任务，如分页器。

```
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}

import { combineReducers, createStore } from 'redux'
let reducer = combineReducers({ visibilityFilter, todos })
let store = createStore(reducer)
```

###redux的四个角色

redux提供了一系列规则来规定我们来写代码。可以大致分为四个角色：

####1.action

action是承载状态的载体，一般action将视图所产出的数据，发送到reducer进行处理。action的书写格式一般是这样：

```
const addAction = {
    type:"ADD",
    value:.....
}
```

action其实就是一个JavaScript对象，它必须要有一个type属性用来标识这个action是干嘛的(也可以认为家的地址，去reducer中找家)，value属性是action携带来自视图的数据。

action的表示方式也可以是一个函数，这样可以更方面的构建action,但这个函数必须返回一个对象。

```
const addAction = (val) => ({
    type:"ADD",
    value: val
})
```

这样拿到的数据就灵活多了。

对于action的type属性，一般如果action变的庞大的话会把所有的type抽离出来到一个constants中，例如：

```
const ADDTODO = 'ADDTODO',
const DELETETODO = 'DELETEDOTO'

export {
    ADDTODO,
    DELETETODO,
}
```

这样可以让type更清晰一些。

####2.reducer

reducer指定了应用状态的变化如何响应 actions 并发送到 store。 在redux的设计原则中提到使用纯函数来编写reducer，目的是为了让state变的可预测。reducer的书写方式一般是这样：

```
 const reducer = (state ={},action){
     switch(action.type){
         case :
            ......
         case :
            ......
         case :
            ......
         default :
            return state;
     }
 }
```

使用switch判断出什么样的action应该使用什么样的逻辑去处理。


拆分reducer

当随着业务的增多，那么reducer也随着增大，显然一个reducer是不可能的，于是必须要拆分reducer，拆分reducer也是有一定的套路的：比如拆分一个TodoList，就可以把todos操作放在一起，把对todo无关的放在一起，最终形成一个根reducer。

```
function visibilityFilter(state,action){
    switch(action.type){
        case :
            ......
        case :
            ......
        default :
            return state;
    }
}
function todos(state,action){
    switch(action.type){
        case :
            ......
        case :
            ......
        default :
            return state;
    }
}
//根reducer
function rootReducer(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action), 
    todos: todos(state.todos, action)
  }
}

```

这样做的好处在于业务逻辑的分离，让根reducer不再那么繁重。好在redux提供了combineReducers方法用于构建rootReducer

```
const rootReducer = combineReducers({
    visibilityFilter,
    todos,
})
```

这部分代码和上面rootReducer的作用完全相同。它的原理是通过传入对象的key-value把所有的state进行一个糅合


####3.dispatch

dispatch的作用是派发一个action去执行reducer。我觉得dispatch就是一个发布者，和subscribe一起组合成订阅发布者模式。使dispatch派发：

```

const action = {
    type: "ADD",
    value: "Hello Redux",
}
dispatch(action);

```

####4.store

store可以说是redux的核心了。开头也提到store是redux状态管理的唯一数据源，除此之外，store还是将dispatch、reducer等联系起来的命脉。

store通过redux提供的createStore创建，它是一个对象，有如下属性：

store.getState() 获取状态的唯一途径
store.dispatch(action) 派发action响应reducer
store.subscribe(handler) 监听状态的变化

创建store：

```
const store = Redux.createStore(reducer,initialState,enhancer);
//1. reducer就是我们书写的reducer
//2. initialState是初始化状态
//3. enhancer是中间件
```

###Middleware

在创建store的时候createStore是可以传入三个参数的，第三个参数就是中间件，使用redux提供的applyMiddleware来调用，applyMiddleware相当于是对dispatch的一种增强，通过中间件可以在dispatch过程中做一些事情，比如打logger、thunk(异步action)等等。

使用方式如下：

```
//异步action中间件
import thunk from "redux-thunk";
const store = Redux.createStore(reducer,initialState,applMiddleware(thunk));
```

学习先告一段落，既然懂得了redux的思想（参考：https://redux.js.org/basics/usage-with-react，https://redux.js.org/basics/usage-with-react），那么接下来探索并手写一个简易版的redux。


##手写一个min-Redux

###createStore

要想了解redux，必然要先了解它的核心，它的核心就是createStore这个函数，store、getState,dispatch都在这里产出。我个人觉得createStore是一个提供一系列方法的订阅发布者模式：通过subscribe订阅store的变化，通过dispatch派发。那么下面就来实现一下这个createStore。

从上面store中可以看出。创建一个store需要三个参数；

```
/1.接受的rootReducer
//2.初始化的状态
//3.dispatch的增强器(中间件)
const createStore = (reducer,initialState,enhancer) => {
    
};
```

createStore还返回一些列函数接口提供调用

```
const crateStore = (reducer, initialState, enhancer) => {
    
    return {
        getState,
        dispatch,
        subscribe,
        replaceReducer,
    }
}
```

####getState的实现

```
let state = initialState;
const getState = () => {
    return state;
}
```

####subscribe的实现

subscribe是createStore的订阅者，开发者通过这个方法订阅，当store改变的时候执行监听函数。subscribe是典型的高阶函数，它的返回值是一个函数，执行该函数移除当前监听函数。

```
//创建一个监听时间队列
let subQueue = [];

const subscribe = (listener) => {
    //把监听函数放入到监听队列里面
    subQueue.push(listener);
    return () => {
        //找到当前监听函数的索引
        let idx = subQueue.indexOf(listener);
        if(idx > -1){
            //通过监听函数的索引把监听函数移除掉。
            subQueue.splice(idx,1);
        }
    }
}
```

####dispatch的实现

dispatch是createStore的发布者，dispatch接受一个action，来执行reducer。dispatch在执行reducer的同时会执行所有的监听函数(也就是发布)。

```
let currentReducer = reducer;
let isDispatch = false;
const dispatch = (action) => {
    //这里使用isDispatch做标示，就是说只有当上一个派发完成之后才能派发下一个
    if(isDispatch){
        throw new Error("dispatch error");
    }
    try{
        state = currentReducer(state,action);
        isDispatch = true;
    }finally{
        isDispatch = false;
    }
    
    //执行所有的监听函数
    subQueue.forEach(sub => sub.apply(null));
    return action;
}
```

####replaceReducer

replaceReducer顾名思义就是替换reducer的意思。再执行createState方法的时候reducer就作为第一个参数传进去，如果后面想要重新换一个reducer，来代码写一下。

```
const replaceReducer = (reducer) => {
    //传入一个reduce作为参数，把它赋予currentReducer就可以了。
    currentReducer = reducer;
    //更该之后会派发一次dispatch，为什么会派发等下再说。
    dispatch({type:"REPLACE"});
}
```

####dispatch({type:”INIT”});

上面已经实现了createStore的四个方法，剩下的就是replaceReducer中莫名的派发了一个type为REPLACE的action，而且翻到源码的最后，也派发一个type为INIT的action，为什么呢？

其实当使用createStore创建Store的时候，我们都知道，第一个参数为reducer，第二个参数为初始化的state。当如果不写第二个参数的时候，我们再来看一下reducer的写法

```
const reducer = (state = {}, action){
    switch(action.type){
        default:
            return state;
    }
}
```

一般在写reducer的时候都会给state写一个默认值，并且default出默认的state。当createStore不存在，这个默认值如何存储在Store中呢？就是这个最后派发的type:INIT的作用。在replaceReducer中派发也是这个原因，更换reducer后派发。

####完整的createStore

```
/**
 * 
 * @param {*} reducer   //reducer
 * @param {*} initState    //初始状态
 * @param {*} middleware   //中间件
 */
const createStore = (reducer, initState,enhancer) => {

    let initialState;       //用于保存状态
    let currentReducer = reducer;        //reducer
    let listenerQueue = []; //存放所有的监听函数
    let isDispatch = false;

    if(initState){
        initialState = initState;
    }

    if(enhancer){
        return enhancer(createStore)(reducer,initState);
    }
    /**
     * 获取Store
     */
    const getState = () => {
        //判断是否正在派发
        if(isDispatch){
            throw new Error('dispatching...')
        }
        return initialState;
    }

    /**
     * 派发action 并触发所有的listeners
     * @param {*} action 
     */
    const dispatch = (action) => {
        //判断是否正在派发
        if(isDispatch){
            throw new Error('dispatching...')
        }
        try{
           isDispatch = true;
           initialState = currentReducer(initialState,action);
        }finally{
            isDispatch = false;
        }
        //执行所有的监听函数
        for(let listener of listenerQueue){
            listener.apply(null);
        }
    }
    /**
     * 订阅监听
     * @param {*} listener 
     */
    const subscribe = (listener) => {
        listenerQueue.push(listener);
        //移除监听
        return function unscribe(){
            let index = listenerQueue.indexOf(listener);
            let unListener = listenerQueue.splice(index,1);
            return unListener;
        }
    }

    /**
     * 替换reducer
     * @param {*} reducer 
     */
    const replaceReducer = (reducer) => {
        if(reducer){
            currentReducer = reducer;
        }
        dispatch({type:'REPLACE'});

    }
    dispatch({type:'INIT'});
    return {
        getState,
        dispatch,
        subscribe,
        replaceReducer
    }
}

export default createStore;`
```



