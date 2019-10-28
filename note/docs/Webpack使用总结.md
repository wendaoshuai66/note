#Webpack使用总结

##前端的发展

近年来 Web 应用变得更加复杂与庞大， Web 前端技术的应用范围也更加广泛。通过直接编写 JavaScript、css、 HTML 开发 Web 应用的方式己经无法应对当前 Web 应用的发展 。 近年来，前端社区涌现出 许 多新思想与框架，下面将一一介绍它 们 。

###模块化

模块化是指将一个复杂的系统分解为多个模块，方便编码

很久以前，开发网页要通过命名空间的方式来组织代码，例如 jQuery 库将它的 API 都放 在了 window .$下，在加载完 jQue町后，其他模块再通过 window . ♀去使用 jQuery。这样做 有很多问题，其中包括:


命名空间冲突，两个库可能会使用同一个名称，例如 Zepto (http://zeptojs.com)也 被放在 w工ndow.$下

无法合理地管理项目的依赖和版本 

无法方便地控制依赖的加载顺序。

直白一些就是开发效率会低，维护成本会大。


####CommonJS

CommonJS是一种广泛应用的Javascript模块化规范，核心：

require方法同步加载依赖的其他模块，

module.exports导出需要的接口

CommonJS的流行 的益于node采用该方式，后来这种方式被引入到网页中开发


采用 CommonJS 导入及导出的代码如下:

```
// 导入
const moduleA = require ( ’. / moduleA ’); // 导出
module .exports = moduleA.someFunc;
```

CommonJS 的优点在于:

1.代码可复用于Node.js环境中，例如做同构

2.通过npm发布的很多第三方模块都采用了，CommonJS规范。

缺点：

1.不能直接在浏览器环境中运行，必须通过工具转换为ES5 的标准


历史

CommonJS 还可以细分为 CommonJSl 和 CommonJS2

CommonJS1 和 CommonJS2区别：

CommonJS1 只能通过 exports . XX = XX 导出


CommonJS2 在 CommonJSl 的基础上加入了 module. exports = XX


####AMD

AMD也是一种 JavaScript 模块化规范。

AMD 与 CommonJS区别：AMD采用了异步的方式去加载依赖的模块。

AMD 主要用于解决针对浏览器环境的模块化问题，最具代表性的实现是requirejs。

采用 AMD 导入及导出的代码如下:

```
采用 AMD 导入及导出的代码如下:
// 定义一个模块
define (’ module ’ , [ ’ dep ’] , function (dep) {
requirejs
(http://
return exports;
});
//导入和使用
require ([’module’] , });
```

AMD 的优点在于:

1.可在不转换代码的情况下直接在浏览器环境中运行

2.异步加载依赖

3.可以并行加载多个依赖

4.代码可以在浏览器环境中与Node环境中运行



AMD 的优点在于:


JavaScript运行环境没有原生支持 AMD。， 需要先导入实现了 AMD 的库 后才能正常使用。


####ES6模块化

ES6 模块化是国际标准化组织 ECMA 提出的 JavaScript 模块化规范，它在 语言层面上实 现了模块化。浏览器厂商和 Node. 都宣布要原生支持该规范 。 它将逐渐取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案 。

采用 ES6 模块化导入及导出的代码如下:

```
//导 入
import { readFile } from ’ fs ’; import React from ’react’; //导出
export function hello {) {};

export default {

...
}
```

### 常见的构建工具及对比

####Gulp


Gulp 是基于流的自动化构建工具。可以管理和执行任务。还可以支持监听文件，与读写文件


Gulp 被设计得非常简单，只通过下面 5 种方法就可以支持几乎 所有构建场景:

1.gulp.task 注册任务

2. gulp.run 执行任务

3.gulp.watch监听任务

4.gulp.src 读取文件

5.gulp.dest写文件

举个例子 Gulp和Rollup实现了对数据的流清洗

```
const gulp = require('gulp');
const watch = require('gulp-watch');
const rollup = require('gulp-rollup');
const babel = require('gulp-babel');
const replace = require('rollup-plugin-replace')
const entry = './src/server/**/*.js';
const clearEntry = ["./src/server/config/index.js"]


function buildenv() {
    return watch(entry, { ignoreInitial: false }, function() {
            gulp.src(entry)
                .pipe(babel({
                    //非常重要一点，涉及到自己独立的编译，外面的留给前端
                    babelrc: false,
                    "plugins": [
                        ["@babel/plugin-proposal-decorators", { "legacy": true }],
                        "@babel/plugin-transform-modules-commonjs"
                    ]
                })).pipe(gulp.dest('dist'))
        })
        // .pipe(gulp.dest('dist')); 放外面监测不到，有点毛病 😔

}
//与开发没有多大区别，只不过要加一个流式清洗
function buildprod() {

    return gulp.src(entry)
        .pipe(babel({
            //非常重要一点，涉及到自己独立的编译，外面的留给前端
            babelrc: false,
            ignore: clearEntry,
            "plugins": [
                ["@babel/plugin-proposal-decorators", { "legacy": true }],
                "@babel/plugin-transform-modules-commonjs"
            ]
        })).pipe(gulp.dest('dist'))
}

function buildconfig() {
    return gulp.src(entry)
        .pipe(rollup({
            //注意一点要编译成commonjs
            output: {
                format: "cjs"
            },
            input: clearEntry,
            //由于打包后，对process.env.NODE_ENV没有处理
            plugins: [
                replace({
                    "process.env.NODE_ENV": JSON.stringify('production')
                })
            ]
        }))
        .pipe(gulp.dest('./dist'));
}

function lint() {

}
let build = gulp.series(buildenv)

if (process.env.NODE_ENV == 'lint') {
    build = gulp.series(lint)
}
if (process.env.NODE_ENV == 'production') {
    build = gulp.series(buildprod, buildconfig)
}
gulp.task("default", build)
```

####Webpack

Webpack 是一个打包模块化js的工具🔧，在Webpack中一切皆模块，通过loader转化文件，通过Plugin注入钩子，最后输出由多个模块组合成的文件📃。

一切文件如 JavaScript、 css、 scss、图片、模板，对于 Webpack来说都是一个个模块


####Rollup

Rollup (https://rollupjs.org)是一个和 Webpack很类似但专注于 ES6 的模块打包工具。它 的亮点 在于，能针对 ES6 源码进行 Tree Shaking，以去除那些己被定义但没被使用的代码 并进 行 Scope Hoisting，以减小输出 文件的大 小和提升运行性能 。然而 Rollup 的这些亮点随后就被 Webpack 模仿和实现。由于 Rollup 的使用方法和 Webpack 差不多 ，所以这里就不详细介绍如 何使用 Rollup 了，而是详细说明它们的差别:


• Rollup 是在 Webpack 流行后出 现的替代品

• Rollup 生态链还不完善，体验不 如 Webpack
• Rollup 的功能不如 Webpack完善 ，但其配置和使用更简单

• Rollup不支持 CodeSpliting，但好处是在打包出来的代码中没有 Webpack那段模块 的加载、执行和缓存的代码







