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

Webpack是一个模块打包工具，在Webpack里一切文件皆模块。通过loader转换文件，通过plugin注入钩子，最后输出由多个模块组合的文件。Webpack专注构建模块化项目。

Webpack可以看作是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其他一些不能被浏览器直接运行的扩展语音(如：Scss,TypeScript等)，并将其打包为合适的格式以供浏览器使用。

![webpack](https://wendaoshuai66.github.io/study/note/images/webpack.png)


###Webpack与Grunt、Gulp的不同？

Grunt/Gulp是一种能够优化前端开发流程的工具，而Webpack是一种模块化的解决方案。


###工作方式不同：

Grunt/Gulp的工作方式是：在一个配置文件中，指明某些文件进行类似编译/组合/压缩等任务的具体步骤，之后工具可以自动帮你完成这些任务

Webpack的工作方式是：把项目当作是一个整体，通过指定的入口文件，Webpack会从这个入口文件开始找到项目所有的依赖文件，然后使用loader处理它们，最后打包成一个或多个浏览器能够识别的JavaScript文件

###构建思路不同

Grunt/Gulp需要将整个前端构建过程拆分成多个task，合理控制所有task的调用关系

Webpack需要定义好入/出口，并需要清楚对于不同类型资源应该用什么loader解析编译


Grunt/Gulp是基于任务和流(task和stream)的。类似jQuery,找到一个(或一类)文件，对其做一系列的链式操作，更新流上的数据，整条链式操作构成了一个任务，多个任务就构成了整个Web的构建流程。


Webpack是基于入口的。Webpack会自动的递归解析入口所需要加载的所有资源文件，然后用不同的loader来处理不同的文件，用pulgin扩展Webpack功能。

###背景知识不同

Grunt/Gulp更像是后端开发者的思路，需要对整个流程了如指掌。Webpack更倾向于前端开发者的思路。


##Webpack的构建流程是怎么样的？

Webpack的运行流程是一个串行的过程，从启动到结束会依次执行以下步骤：

1.初始化参数：从配置文件和shell语句中读取与合并参数，得到最终参数；

2.开始编译：用上一步得到的参数初始化Compiler对象，加载所有配置的插件，执行对象的run方法开始执行编译；

3.确定入口：根据配置中的entry找出所有的入口文件；

4.编译模块：从入口文件出发，调用所有配置的loader对模块进行编译。再找出该模块依赖的模块，再递归本步骤，直到所有入口依赖的文件都经过本步骤的处理；

5.完成模块编译：在经过第四个步骤使用loader编译完所有模块后，得到每个模块被编译后的最终内容以及它们之间的依赖关系；

6.输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的Chunk，再把每个Chunk转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会；

7.输出完成：在确认好输出内容后，根据配置确定输出的路径和文件名，把文件内容写进到文件系统中；


在以上过程中，Webpack会在特定的的时间点广播特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑。并且插件可以调用Webpack提供的API改变Webpack的运行结果。


##分别介绍bundle，chunk，module是什么？

bundle：由Webpack打包出来的文件

chunk：代码块，webpack在进行模块的依赖分析的时候，代码分割出来的代码块

module：是开发中的单个模块，在Webpack中，一切皆模块，一个模块对应一个文件

##什么是Loader?什么是Plugin?

loader: 模块转换器，用于对模块的源代码进行转换

plugin: 自定义webpack打包过程的方式，插件含有apply属性的JavaScript对象，apply属性会被webpack compiler调用，并且compiler对象可以在整个编译生命周期内访问

##loader和plugin有哪些不同？

###不同的作用

loader直译为“加载器"，Webpack将一切文件视为模块，但是Webpack原生只能解析JavaScript和JSON类型文件。如果想加载解析其他类型文件，就会用到loader。所以loader是让Webpack拥有加载和解析非JavaScript文件的能力

plugin直译为”插件“，plugin可以扩展Webpack的功能，让Webpack具有更多的灵活性。在Webpack运行的生命周期中会广播许多事件，plugin可以监听这些事件，在合适的时机通过Webpack提供的API改变输出结果


###不同的用法

loader在module rules中配置，也就说它作为模块解析规则存在。类型为Array，每一项都是一个Object，里面描述了什么类型的文件(test)，使用什么加载(loader)和使用的参数(options)



plugin单独在plugins中单独配置。类型为Array，每项都是一个plugin的实例，参数是通过构造函数传入

##有哪些常见的Loader？

file-loader: 将文件输出到一个文件夹中，在代码中通过相对路径(url)去引用输出的文件

url-loader: 和file-loader类似，但是能在文件很小的情况下，以base64的方式将内容注入到代码中

image-loader: 加载并压缩图片文件

babel-lodader: 将ES6转成ES5

css-loader: 加载CSS，支持模块化/压缩/文件导入等特性

style-loader：把CSS代码注入到JavaScript中，通过DOM操作去加载CSS

eslint-loader: 通过ESlint检查JavaScript代码

##有哪些常见的Plugin？

define-plugin: 定义环境变量

html-webpack-pulgin: 生成创建html入口文件，并引用对应的外部资源

uglifyjs-plugin: 通过Uglifyjs压缩JavaScript代码

mini-css-extract-plugin: 分离CSS文件

clean-webpack-plugin: 删除打包文件

happypack: 实现多线程加速编译

##Tree Shaking

为了使用tree shaking，需要满足以下条件：

1.使用ES2015语法(即import和export)

2.在项目package.json文件中，添加sideEffects入口

3.引入一个能够删除未引用代码(dead code)的压缩工具(minifier)(例如：UglifyJSPlugin)

###将文件标记为无副作用(side-effect-free)

这种方式是通过package.json的sideEffects属性来实现的。

```
{
  "sideEffects": false
}
```

「副作用」的定义是，在导入时会执行特殊行为的代码，而不是仅仅暴露一个export或多个export。举例说明，例如polyfill，它影响全局作用域，并且通常不提供export。


注意，任何导入的文件都会受到tree shaking的影响。这意味着，如果在项目中使用类似css-loader并导入CSS文件，则需要将其添加到 side effect 列表中，以免在生产模式中无意中将它删除：

```
{
  "sideEffects": ['*.css']
}
```

###压缩输出

从 webpack 4 开始，也可以通过 "mode" 配置选项轻松切换到压缩输出，只需设置为 "production"。

也可以在命令行接口中使用--optimize-minimize标记，来使用UglifyjsPlugin。

## Code Splitting

code splitting的必要性

➡️不进行code splitting，打包后单文件提交较大，加载时长较长，影响用户体验


➡️不进行code splitting，经常修改业务代码，重新打包后，浏览器不能进行缓存，导致性能较差，影响用户体验

### 同步代码

````
import _ from 'lodash';
````

webpack.common.js配置如下：

```
....
optimization: {
  splitChunks: {
    chunks: 'all'
  }
}
....
```

配置后，会将公用类库进行打包，生成一个vendors~main.js文件。

###异步代码

```
function getComponent() {
  return import('lodash').then(({ default: _ }) => {
    var element = document.createElement('div');
    element.innerHTML = _.join(['Clear', 'love'], '');
    return element;
  })
}

getComponent().then(element => {
  document.body.appendChild(element);
})
```

##webpack-dev-server和http服务器如nginx有什么区别？

webpack-dev-server使用内存来存储Webpack开发环境下打包的文件，并且可以使用模块热更新，它比传统的http服务对开发更加简单高效。

##什么 是模块热更新？

模块热更新是Webpack是的一个功能，它可以使得代码修改以后不需刷新浏览器就可以更新，是高级版的自动刷新浏览器。devServer通过hot属性可以控制模块热更替。

##通过配置文件

```
const webpack = require('webpack');
const path = require('path');
let env = process.env.NODE_ENV == "development" ? "development" : "production";
const config = {
  mode: env,
  devServer: {
     hot:true
  },
  plugins: [
     new webpack.HotModuleReplacementPlugin(), //热加载插件
  ]
}
module.exports = config;
```

###通过命令行

```
"script": {
  "start": "NODE_EVN=development webpack-dev-server --config webpack-devlop-config.js --hot"
}
```



