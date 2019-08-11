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


###JSX注意事项

1.React的JSX里约定分别使用首字母大、小写来区分本地组件的类和HTML标签。并使用驼峰命令。

自定义组件：List，HTML标签：html、htmlMessage

2.要使用JavaScript表达式作为属性值，只需要把这个表达式用一对大括号（{}）包起来，如果直接写值是需要用包起来。不需要用引号（“”）

如果需要写多行就需要写在()里面，多行书写JSX语法，只能有一个跟标签。 例如：

```
return(<div>
          <span></span>  
        </div>)
```


3.htmlFor和className

在html中label使用for属性、JSX中使用htmlFor，html中css类使用class，JSX中使用className

4.CSS in JS （React style)

JSX允许在js中书写css，相当于js中的一个对象

```
const style = {
     color:'red',
     fontSize : 38,
 }
 <div style={style}> <div/>
```

5.JSX里需要加注释很容易，他们只是js表达式而已。只需要在一个标签的子节点内(非最外层)小心的用{}包围要注释的范围，在JSX中写注释和平常是有一些区别。

{/* 注释内容*/}


6.事件绑定

```
handleChange(e){
         console.log(e.target)
     }
     render(){
         return (<input onClick = {this.handleChange.bind(this)}/>)  //需要改变this    
     }
        
```



##DOM diff

DOM diff是DOM比较算法。用于找到最小变化的DOM元素进行渲染，平行化比较，把重绘和重排做到最小化。

react中View层的变化是基于state的，如果状态发生变化，组件中render函数就会重新执行。


###流程图

![](https://wendaoshuai66.github.io/study/note/images/domedif流程图.png)

###DOM diff算法流程

1.开始判断节点是否相同，如果节点不相同，就相当于修改了节点，React重新创建一个节点；

2.如果节点相同，判断是否是自定义节点；

3.如果不是自定义节点(ReactDOM.render也可以渲染html标签)，比较属性是否发生变化，如果属性变更新属性然后结束；

4.如果是自定义节点，或者说是我们自己写的组件，然后重新渲染，会渲染出一段VirtualDOM（虚拟DOM）,然后和以存在的Virtual DOM进行比较区别，最终渲染到页面。


##非DOM属性的介绍

###dangerouslySetInnerHTML警告

dangerouslySetInnerHTML用于净化数据，预防XSS的攻击

```
let rawHTML = {
    //通过__html来构造出来，
    __html: "<h2>非dom属性：dangerouslySetInnerHTML标签</h2>"
}

 <div dangerouslySetInnerHTML={rawHTML}>

 </div>
```

###ref

如果在JSX中获取真正的DOM元素，可以使用ref这个属性。

在html元素中添加ref属性


```
<input type="text" ref= 'input'/>
```

如果想要取得这个DOM元素可以通过ReactDOM.findDOMNode()来获取,并且需要在React生命周期的componentDidMount阶段

```
ReactDOM.findDOMNode(this.refs.input)   //获取到input元素

```

###key提高渲染的性能

key帮助React识别哪些项目已更改，已添加或已删除。应该为数组内部的元素赋予键，以使元素具有稳定的标识：key必须在唯一的

```
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>
    {number}
  </li>
);
```
[参考官网](https://reactjs.org/docs/lists-and-keys.html#keys)

[关于key讨论](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/1)


注意：另外有个方式：推荐使用[shortid](https://github.com/dylang/shortid)生成唯一key的数组，和数据数组一起使用，省去提交数据时再重组数组。

案例：

```
import React from 'react';
import shortid from 'shortid';

class Demo extends React.Component {
    constructor(props) {
    super(props);
    this.state = {
      data: ['a', 'b', 'c']
    }
    this.dataKeys = this.state.data.map(v => shortid.generate());
  }
  
    deleteOne = index => { // 删除操作
        const { data } = this.state;
        this.setState({ data: data.filter((v, i) => i !== index) });
        this.dataKyes.splice(index, 1);
    }
    
    render() {
      return (
          <ul>
               {
                   data.map((v, i) => 
                    <li 
                        onClick={i => this.deleteOne(i)}  
                        key={this.dataKeys[i]}
                    >
                        {v}
                    </li>
                    )
               } 
            </ul>
      )
  }
}
// 稍微抽取，可以封装一个通用的组件复制代码

```


##React props and state

###Props

Props(properties)：属性，一个事物性质与关系，可以说组件是React的核心了，如果把组件比喻成一个管道，那么props就相当于输入。

props可以定义在注册
件的地方，也可以在组件内部定义默认属性，无论在哪里定义，props都是只读的。

props可以应用于JSX中html的元素上，自定义组件的元素上(相当于给子组件传值)，也可以应用于值。



###定义默认props

ES5和ES6定义默认的props是不相同的。

ES5

```
var Hello = React.createClass({
    getDefaultProps : function(){   //设置默认属性
         return { title : '133'};
    }
    propTypes : { //属性校验器，表示必须是string
        title : React.PropTypes.string,
    }  
}) 
```

上面使用getDefauktProps定义属性，propTypes用于属性的类型检查。


ES6

ES6同样有两种方法，由于ES6是使用class类来定义组件的，因此，这两种方法必须是静态。


```

export default class Hello extends React.Component{
    static defaultProps ={
        title : "Hello React",
    }
    static propTypes = {
        title : React.PropTypes.string,
    }
}

```

运行上面代码会发现报错。报错原因在React.PropTypes.string，这是因为在React15.5之前类型检查是集成在React里面的，React15.5之后被抽离了出来。所以需要下载prop-types包来解决这个问题。

```
npm install prop-types –save

```

再修改一下代码。

```
import propTypes from "prop-types";
export default class Hello extends React.Component{
    static defaultProps ={
        title : "Hello React",
    }
    static propTypes = {
        title :propTypes.string,
    }
}
```

这时候运行就不会报错了。

###state

state在React中是状态，是组件自身所拥有的东西，并且可以自己设置和改变。

在开发过程中的状态都是我们自己来维护的，比如说发一个请求，请求发送成功会怎么样、失败会怎么样等等。

React是基于状态的，就是在代码中定义了状态，只要在任何地方改变了状态，最初定义状态的地方就会发生改变。


###setState

React如果想改变一个状态，那么必须通过setState切换撞他，每一次setState之后，Reader就会重新渲染执行一次render，就会触发diff算法进行计算，通过计算生出新的Virtual Dom和现在的Virtual DOM进行比较，发生变化之后执行一次更新。

###使用state

state在ES5和ES6上都是不同的。

ES5

通过getInitialState方法来初始化状态。

```
var Hello = React.crateClass({
    getInitialState : function(){
        return {
            isloading : false,
        }
    }
})

```

ES6

```
export default class Hello extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            isloading : false,
        }
    }
}
```

上面两种方式同样是通过this.setState()改变状态。

以ES6为例

###Props 和State的比较

相同点

1.都是纯的JS对象，都包含这一些信息。

2.都会触发render更新；属性是开始渲染一次性触发render，状态是每次状态改变都会触发render

3.都具有确定性，渲染前初始化完成

区别

属性只传递一次，状态是不停的在更新。组件在运行时需要修改的就是状态，属性在组件内运行时是修改不了的。

比较
 
<table>


<th>

<td> Props	</td>
<td> State </td>
</th>


<tr>
<td>能否从父组件获取初始值？</td>
<td > 能 </td>
<td > 不能</td>
</tr>


tr>
<td>能否由父组件修改？</td>
<td > 能 </td>
<td > 不能</td>
</tr>

tr>
<td>能否在组件内部设置默认值？</td>
<td > 能 </td>
<td > 能</td>
</tr>

tr>
<td>能否在组件内部修改？</td>
<td > 不能 </td>
<td > 能</td>
</tr>

tr>
<td>能否设置子组件的初始值？</td>
<td > 能 </td>
<td > 不能</td>
</tr>

tr>
<td>能否修改子组件的值？ ？</td>
<td > 能 </td>
<td > 不能</td>
</tr>


</table>








