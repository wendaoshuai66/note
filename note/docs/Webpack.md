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

##Webpack的热更新是如何做到的？说明其原理？

Webpack的热更新有称为热替换(Hot Module Replacement)，缩写为HMR。这个机制可以实现不刷新浏览器而将新变更的模块替换旧的模块。原理如下：

![](https://wendaoshuai66.github.io/study/note/images/hmr.jpeg)


###server端和client端都做了哪些具体工作：

1.第一步，在Webpack的watch模式下，文件系统中某一个文件发生修改，Webpack监听到文件变化，根据配置文件对模块重新编译打包，并将打包后的代码通过简单的JavaScript对象保存在内存中。

2.第二步是Webpack-dev-server和Webpack之间的接口交互，而在这一步，主要是dev-server的中间件Webpack-dev-middleware和Webpack之间的交互，Webpack-dev-middleware调用Webpack暴露的API对代码变化进行监控，并且告诉webpack，将代码打包到内存中。

3.第三步是Webpack-dev-server对文件变化的一个监控，这一步不同于第一步，并不是监控代码变化重新打包。当我们在配置文件中配置了devServer.watchContentBase为true的时候，Server会监听这些配置文件夹中静态文件的变化，变化后会通知浏览器端对应用进行live reload。注意，这儿是浏览器刷新，和HMR是两个概念。

4.第四步也是webpack-dev-server代码的工作，该步骤主要是通过sockjs（webpack-dev-server 的依赖）在浏览器端和服务端之间建立一个websocket长连接，将Webpack编译打包的各个阶段的状态信息告知浏览器端，同时也包括第三步中Server监听静态文件变化的信息。浏览器端根据这些socket消息进行不同的操作。当然服务端传递的最主要信息还是新模块的hash 值，后面的步骤根据这一hash值来进行模块热替换。

5.webpack-dev-server/client端并不能够请求更新的代码，也不会执行热更模块操作，而把这些工作又交回给了Webpack，webpack/hot/dev-server的工作就是根据webpack-dev-server/client传给它的信息以及dev-server的配置决定是刷新浏览器呢还是进行模块热更新。当然如果仅仅是刷新浏览器，也就没有后面那些步骤了。

6.HotModuleReplacement.runtime是客户端HMR的中枢，它接收到上一步传递给他的新模块的hash值，它通过JsonpMainTemplate.runtime向server端发送Ajax请求，服务端返回一个json，该json包含了所有要更新的模块的hash值，获取到更新列表后，该模块再次通过jsonp请求，获取到最新的模块代码。这就是上图中 7、8、9 步骤。

7.而第 10 步是决定HMR成功与否的关键步骤，在该步骤中，HotModulePlugin将会对新旧模块进行对比，决定是否更新模块，在决定更新模块后，检查模块之间的依赖关系，更新模块的同时更新模块间的依赖引用。

8.最后一步，当HMR失败后，回退到live reload操作，也就是进行浏览器刷新来获取最新打包代码。


##如何提高webpack的构建速度?

###常规

####保持版本最新

使用最新稳定版本的webpack、node、npm等，较新的版本更够建立更高效的模块树以及提高解析速度。

#### loaders

由于loader对文件的转换操作很耗时，所以需要让尽可能少的文件被loader处理。我们可以通过以下3方面优化loader配置：

优化正则匹配

通过cacheDirectory选项开启缓存

通过include、exclude来减少被处理的文件

```
// webpack.common.js
module: {
    rules: [
        {
            test:/\.js$/,
            //babel-loader支持缓存转换出的结果，通过cacheDirectory选项开启
            loader:'babel-loader?cacheDirectory',
            //只对项目根目录下的src 目录中的文件采用 babel-loader
            include: [path.resolve('src')],
            //排除 node_modules 目录下的文件，node_modules 目录下的文件都是采用的 ES5 语法，没必要再通过 Babel 去转换
            exclude: path.resolve(__dirname, 'node_modules')
        }
    ]
}

```

####optimization.splitChunks 提取公共代码

Webpack 4移除了CommonsChunkPlugin取而代之的是两个新的配置项optimization.splitChunks和optimization.runtimeChunk来简化代码分割的配置。

通过设置 optimization.splitChunks.chunks: "all" 来启动默认的代码分割配置项。

当满足如下条件时，webpack 会自动打包 chunks:

➡️当前模块是公共模块（多处引用）或者模块来自node_modules

➡️当前模块大小大于30kb, 如果此模块是按需加载，并行请求的最大数量小于等于5

➡️如果此模块在初始页面加载，并行请求的最大数量小于等于 3

```
optimization: {
    splitChunks: {
        chunks: 'async', // all async initial 是否对异步代码进行的代码分割
        minSize: 30000,  // 引入模块大于30kb才进行代码分割
        maxSize: 0, // 引入模块大于Xkb时，尝试对引入模块二次拆分引入
        minChunks: 1, // 引入模块至被使用X次后才进行代码分割
        maxAsyncRequests: 5, // 
        maxInitialRequests: 3,
        automaticNameDelimiter: '~', // 模块间的连接符，默认为"~"
        name: true,
        cacheGroups: {
            vendors: {
                test: /[\\/]node_modules[\\/]/,
                priority: -10  // 优先级，越小优先级越高
            },
            default: {  // 默认设置，可被重写
                minChunks: 2,
                priority: -20,
                reuseExistingChunk: true  // 如果本来已经把代码提取出来，则重用存在的而不是重新产生
            }
        }
    }
}
```

#### Smaller = false

减少编译的整体大小，以提高构建性能。尽量保持chunks小巧。

➡️使用更小/更少的库

➡️移除不需要的代码

➡️只编译你在开发的代码

#### Worker Pool

thread-loader可以将非常耗性能的loaders转存到worker pool中。

不要使用太多的workers，因为Node.js的runtime和loader有一定的启动开销。最小化workers和主进程间的模块传输。进程间通讯(IPC)是非常消耗资源的。

####持久化缓存


对于一些性能开销较大的loader之前可以添加cache-loader，启用持久化缓存。

使用package.json中的postinstall清楚缓存目录。

####Dlls

使用DllPlugin将更新不频繁的代码进行单独编译。这将改善引用程序的编译速度。即使它增加了构建过程的复杂度。

利用DllPlugin和DllReferencePlugin预编译资源模块， 通过DllPlugin来对那些我们引用但是绝对不会修改的npm包来进行预编译，再通过DllReferencePlugin将预编译的模块加载进来。

####解析(resolve)

以下几步可以提高解析速度：

➡️  尽量减少resolve.modules、resolve.extensions、resolve.mainFiles、resolve.desciriptionsFiles中类目的数量，因为它们会增加文件系统的调用次数。

➡️ 如果你不使用symlinks，可以设置resolve.symlinks: false

➡️ 如果你使用自定义解析plugins，并且没有指定context信息，可以设置resolve.cacheWithContext: false

### Development

####在内存中编译

以下几个实用的工具通过在内存中进行代码的编译和资源的提供，但并不写入磁盘来提高性能：

webpack-dev-server

webpack-hot-middleware

webpack-dev-middleware

#### Devtool

需要注意在不同的devtool的设置，会导致不同的性能差异。

➡️eval具有最好的性能，但不能帮你转义代码

➡️如果你能接受稍微差一些的mapping质量，你可以使用cheap-source-map选择来提高性能

➡️使用eval-source-map配置进行增量编译 

在大多数情况下，cheap-module-eval-source-map是最好的选择。

####避免在生产环境在才会用到的工具

某些实用工具，plugins和loaders都只能在构建生产环境时才使用。例如，在开发时使用UglifyJsPlugin来压缩和修改代码是没有意义的。以下这些工具在开发中通常被排除在外：

UglifyJsPlugin

ExtractTextPlugin

[hash]/[chunkhash]

AggressiveSplittingPlugin

AggressiveMergingPlugin

ModuleConcatenationPlugin

####最小化入口chunk

webpack只会在文件系统中生成已更新的chunk。应当在生成入口chunk时，尽量减少入口chunk的体积，以提高性能。

###Production

不要为了非常小的性能增益，牺牲了你应用程序的质量！！请注意，在大多数情况下优化代码质量，比构建性能更重要。

####多个编译时

当进行多个编译时，以下工具可以帮助到你：

parallel-webpack: 它允许编译工作在woker池中进行。
cache-loader: 缓存可以在多个编译之间共享。

###工具相关问题

####Babel

项目中的preset/plugins数量最小化

####TypeScript


在单独的进程中使用fork-ts-checker-webpack-plugin进行类型检查
配置loaders时跳过类型检查

使用ts-loader时，设置happyPackMode: true以及 transpileOnly: true

###Saas

node-sass中有个来自Node.js线程池的阻塞线程的bug。当使用thread-loader时，需要设置workParallelJobs: 2

##如何利用Webpack来优化前端性能？（提高性能和体验）

用Webpack优化前端性能是指优化Webpack输出结果，让打包的结果在浏览器运行快速高效。

压缩代码。删除多余的代码/注释，简化代码的写法等等方式。可以利用Webpack的UglifyJsPlugin和ParallelUglifyPlugin来压缩JavaScript代码。利用css-loader?minimize来压缩CSS

压缩图片。利用imagemin-webpack-plugin等图片资源压缩插件，对引用的图片资源进行压缩处理

合理的图片资源引用。使用url-loader加载解析图片资源时，可以通过配置options limit参数，将较小的图片资源转换成base64格式，减少http请求

利用CDN加速。在构建过程中，将引用的静态资源路径修改为CDN上对应的路径。可以利用Webpack对于output参数和各个loader的publicPath参数来修改资源路径

删除死代码(Ttee Shaking)。将代码中没有引用的代码片段删除掉。可以通过在启动Webpack时追加参数--optimize-minimize来实现
提取公共代码

##npm打包时需要注意哪些？如何利用Webpack来更好的构建？

###npm模块需要注意以下问题：

要支持CommonJS模块化规范，所以打包后的最后结果也要支持该规则

npm模块使用者的环境是不确定的，很有可能并不支持ES6，所以打包的最后结果应该是采用ES5编写的。并且如果ES5是经过转换的，请最好连同SourceMap一同上传

npm包大小应该是尽量小（有些仓库会限制包大小）

发布的模块不能将依赖的模块也一同打包，应该让用户选择性的去自行安装。这样可以避免模块应用者再次打包时出现底层模块被重复打包的情况

UI组件类的模块应该将依赖的其它资源文件，例如.css文件也需要包含在发布的模块里

## 基于以上需要注意的问题，我们可以对于Webpack配置做以下扩展和优化：

CommonJS模块化规范的解决方案： 设置output.libraryTarget='commonjs2'使输出的代码符合CommonJS2模块化规范，以供给其它模块导入使用


输出ES5代码的解决方案：使用babel-loader把ES6代码转换成ES5的代码。再通过开启devtool: 'cheap-module-eval-source-map'输出SourceMap以发布调试

npm包大小尽量小的解决方案：Babel在把ES6代码转换成ES5代码时会注入一些辅助函数，最终导致每个输出的文件中都包含这段辅助函数的代码，造成了代码的冗余。解决方法是修改.babelrc文件，为其加入transform-runtime插件

不能将依赖模块打包到npm模块中的解决方案：使用externals配置项来告诉Webpack哪些模块不需要打包

对于依赖的资源文件打包的解决方案：通过css-loader和extract-text-webpack-plugin来实现，配置如下：

```
const ExtractTextPlugin = require('extract-text-webpack-plugin');
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ExtractTextPlugin.extract({ use: ['css-loader'] })  // 提取出chunk中的css到单独的文件中
      }
    ]  
  },
  plugins: [
    new ExtractTextPlugin({ filename: 'index.css' })
  ]
}
```














