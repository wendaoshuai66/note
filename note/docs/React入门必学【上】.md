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


