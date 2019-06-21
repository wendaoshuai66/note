#常用的前端构建工具-Webpack

##前端模块化的打包工具

Webpack前端模块化的打包工具。它将每一个静态文件当做一个模块，经过一系列的处理为我们整合出最后的需要的js、css、图片、字体等文件。

模块化是一种将系统分离成独立的功能部分的方法，严格定义模块接口、模块间具有透明。

###模块化历史

1.无模块时代

全局变量泛滥、命名冲突、依赖关系管理

2.模块萌芽时代

立即执行函数（IIFE）

经典的例子jq：

```
(function(window){


   window.jQuery = window.$ = jQuery;
})(window))
```

3.现代化模块


 (1.)CommonJs (通过require引入)

（2.）RequireJs（AMD）/ SeaJs （CMD） 
[参考](https://www.zhihu.com/question/20351507/answer/14859415)


（3.）ES6 Moudle

## 为什么用webpack

一句玩笑话因为 Vue React 火🔥


###什么是Webpack

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

![webpack](https://wendaoshuai66.github.io/study/note/images/webpack.png)

