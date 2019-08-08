#React入门必学【上】

##什么是React

React是Facebook开发的一款用来构建用户界面的js库，对于react来讲还可以应用到nodejs，做同构化应用。 对于React自己来说，React只做的是非常纯粹的View层。React结合自己庞大的组件库，形成了MVVM框架。


React Native版本，可以用一套程序写IOS、安卓各种客户端。

##React特性

###Virtual DOM

Virtual DOM是一个模拟DOM树的JavaScript对象。React使用Virtual DOM 来渲染UI，同时监听Virtual DOM上数据的变化，并自动迁移到UI上。

###state

React有一个叫做state的概念。state是状态，整个React都是通过状态来驱动的，只要状态变换，React就会驱动View变化，View变化就会启动VirtualDOM的diff算法，通过diff算法找到DOM元素最小的变化，从而实现最小的操作DOM元素。

###props

props是React中的属性，通过属性可以做到父子组件间的通信。


###JSX语法

JSX语法是React定义的一种JavaScript语法扩展，类似于XML。jSX是可选的，在开发过程中也可以不使用JSX，使用JavaScript来编写React应用(建议使用)。

###components

React是专注于View层开发的，View是基于组件的，每一个JSX是一个组件。组件化开发可以创建可复用的UI组件，提高开发效率。


##组件 & Props & 元素渲染


组件可以将UI切分成一些独立的、可复用的部件，这样你就只需专注于构建每一个单独的部件。

组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。



##React JSX语法学习

###JSX语法的介绍

JSX(JavaScript XML) 是基于ECMAScript的一种新特性一种定义带属性树结构的语法。它不是XML或者HTML的一种限制，可以说是对它们的一种。


[官网](https://zh-hans.reactjs.org/docs/introducing-jsx.html)


###JSX语法的书写方式

JSX语法有两种书写方式：一种是ES5的书写方式(现在不适用,还是要记一下吧，以免以后忘了)，一种是ES6的书写方式。

####ES5的方式

ES5使用React.crateClass的方式来构建组件。

```
var Hello = React.createClass({
    render : function(){
        return <h1> {this.props.name}</h1>
    }
})
```

###ES6的方式

ES6采用新版本的Class来构建组件

```
"use strict";
class Hello extends React.Component{
    render(){
        return <h1>Hello</h1>
    }
}
```

上面了两种方式生成出来的叫组件，对应的Hello叫做组件名，简单理解就是HTML标签。但是要怎么注册这两个组件呢？上面两种方式的注册组件的方式相同。


```
ReactDOM.render(
    <Hello name="ESMA" />,
    document.getElementById('app')
)
```


使用ReactDOM.render把我们的组件传进去，然后再传一个DOM节点，相当于把组件里面的东西渲染到id为app的DOM元素里面。

这里传进去了name属性，在组件里面通过使用this.props.name获取。this.props相当于attribute。用于获取书写在组件标签中的属性。

##DOM diff

DOM diff是DOM比较算法。用于找到最小变化的DOM元素进行渲染，平行化比较，把重绘和重排做到最小化。

react中View层的变化是基于state的，如果状态发生变化，组件中render函数就会重新执行。