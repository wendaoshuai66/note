#JavaScript集成化测试
##有关好的文章地址
[JavaScript与QA工程师(理论篇)](https://github.com/hubvue/nota/issues/26)
##yarn 与 npm
![yarn 与 npm](https://wendaoshuai66.github.io/study/note/images/yarn.png)
##单元测试 小的函数
单元测试的测试框架有很多中，今天用的是[karma](http://karma-runner.github.io/3.0/intro/installation.html)，还有一个是React推出的[jest](https://jestjs.io/docs/en/getting-started)
###初始化项目
```
npm init -y //-y 一直yes
```
###下载karma包
```
npm install karam –save-dev 或者 yarn add karam --dev
```

###安装karma-jasmine和jasmine-core断言库
```
npm install karma-jasmine jasmine-core –save-dev
```
###安装浏览器启动项
如果用无头浏览器的话会因为PhantomJs对es6支持不好会造成问题但正常的话可以用

步骤：npm install –save-dev karma-phantomjs-launcher(这个在karma init的时候会下载，可以先不下载，需要翻墙，代理服务器带宽不好的下载不下来。一个解决办法是整个好的代理服务器，二个解决办法是使用cnpm下载)

```
npm install phantom –save-dev
```
因为PhantomJs对es6支持不好本次测试用的是Chrome，需安装Chrome启动项

```
npm install  karma-chrome-launcher --save-dev
```
###初始化karma包

```
karma init
```

执行上面这个命令会有几个选项让选择

which testing framework do you want to use? (你想用什么测试框架) 我们选jasmine。

Do you want to use Require.js ?（用Require.js吗） 按需求来选，如果用的话选yes，不用的话选no

Do you want to capture any browsers automatically ?（是否要进行浏览器的比较） 找浏览器执行ceshi，这里是用的是Chrome，因为PhantomJs对es6支持不好本次测试用的是Chrome

What is tceshihe location of your source and test files ?（测试文件和原文件的位置在什么地方？？） 可选可不选，ceshi可以自己配置。

Should anceshiy of the files included by the previous patterns be excluded ?（是否要有一些其他文件被包含）ceshi 可选可不选，ceshi这里不选

Do you waceshint Karma to watch all the files and run the tests on change ?（你想让Karma监视所有的文件并运行变更测ceshi试吗？） 可选可不选，这里不选

安装完成之后，会生成karma.vonf.js文件，这个就是karma的配置文件。

###karma配置文件

```
module.exports = function(config) {
  config.set({

       basePath: '',//基本路径

       frameworks: ['jasmine'],
		//需要添加测试文件
	    files: [
		  "./tests/unit/**/*.js",
		  "./tests/unit/**/*.spec.js"
		],

    exclude: [
    ],


    // preprocess matching files before serving them to the browser
    // available preprocessors: https://npmjs.org/browse/keyword/karma-preprocessor
    preprocessors: {
      './tests/unit/**/*.js': ['coverage']
    },
    //代码覆盖率文档生成的路径
    coverageReporter: {
      type : 'html',
      dir : './docs/coverage/'
    },

    //生成测试报表的过程
    reporters: ['progress', 'coverage'],


    // web server port
    port: 9876,


    // enable / disable colors in the output (reporters and logs)
    colors: true,


    // level of logging
    // possible values: config.LOG_DISABLE || config.LOG_ERROR || config.LOG_WARN || config.LOG_INFO || config.LOG_DEBUG
    logLevel: config.LOG_INFO,


    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: false,


    // start these browsers
    // available browser launchers: https://npmjs.org/browse/keyword/karma-launcher
    browsers: ['Chrome'],


    // Continuous Integration mode
    // if true, Karma captures browsers, runs the tests and exits
    singleRun: false,

    // Concurrency level
    // how many browser should be started simultaneous
    concurrency: Infinity
  })
}
```

frameworks：是指测试所用到的断言库是什么，我用的是jasmine。

files：是需要测试的文件，配置两中，一种是需要测试的文件，一种是测试文件(.spec.js结尾)。主要**是所有的文件夹， *是所有的文件。

preprocessors：这个是指定什么文件生成报表文件。

reporters：以什么样的方式生成报表。

coverageReporter：生成报表的形式和路径。

browsers：指定测试用到的浏览器，这里用PhantomJS无头浏览器。

singleRun：如果上面browsers指定的是PhantomJS，这里要改为true。

###开始需要添加的配置–指定测试文件的路径

```
 files: [
		  "./tests/unit/**/*.js",
		  "./tests/unit/**/*.spec.js"
		],
```

###编写测试用例

假设我们测试一下代码

```
window.add = (a)=>{
    if(a===1){
        return 1
    }else{
        return a+1;
    }
    
}
```

测试用例这样写

```
describe('测试基本函数函数API',function(){
    it('+1的函数应用',function(){
        expect(window.add(1)).toBe(1)
        // expect(window.add(2)).toBe(3)
    })
})
```

describe：整个项目的描述

it:是每一个测试用例

expect：是断言

toBe：直接的结果与谁相比
###启动测试
```
karma start
```
测试成功的话是绿色的显示SUCCESS

![karma成功](https://wendaoshuai66.github.io/study/note/images/karmas.jpeg)

测试失败的话会报错。

![karma成功](https://wendaoshuai66.github.io/study/note/images/karmae.jpeg)

###代码的覆盖率检查

当测试代码用有ifelse的时候，测试用例中只写一个测试是达不到完成测试的，就上面测试代码而言。我们用代码的覆盖率检查来检测一下。

覆盖率检查通过karma-coverage来完成

```
npm install karma-coverage –save-dev
```

在karma.conf.js中修改 设置以coverage的方式生成报表

```
reporters: ['progress','coverage'],
```

设置处理的文件

```
preprocessors: {
    "unit/**/*.js":['coverage']
}
```

文档生成的位置 在目录下设置docs文件夹存放报表文件


```
 coverageReporter: {
      type : 'html',
      dir : './docs/coverage/'
    },
```

此时生成的报表文件

![报表成功](https://wendaoshuai66.github.io/study/note/images/reports.png)

可以看到Branches是50%，说明只能测试到了50%的代码 我们修改测试代码

```

describe('测试基本函数函数API',function(){
    it('+1的函数应用',function(){
        expect(window.add(1)).toBe(2)
        expect(window.add(2)).toBe(3)
    })
})
```

###此时的测试用例
![报表成功100%](https://wendaoshuai66.github.io/study/note/images/reporte.png)

##功能测试
###e2e测试

e2e主要测浏览器的功能测试 e2e主要使用的是selenium-webdriver，这里是它其他的一些方法API

###安装selenium-webdriver
```
npm install selenium-webdriver –save-dev
```
selenium-webdriver通过自动启动浏览器进行e2e测试，因此必须要安装浏览器驱动程序。 在这里下载各大浏览器的驱动程序，把下载好的驱动程序解压下来放在项目根路径下就可以了。

###测试流程
比如说我们要测试百度的input框输入内容之后跳转的相应的界面中的title值。 在项目根目录中创建e2e文件夹，创建baidu.spec.js文件，这个baidu.spec.js文件就是测试文件。测试文件中代码如下。

```
const {Builder, By, Key, until} = require('selenium-webdriver');
(async function example() {
    //一定要下载相对应浏览器的执行驱动
    //forBrowser里就是指定用什么样的浏览器进行测试。
    let driver = await new Builder().forBrowser('firefox').build();
    try {
        //需要测试网站的url
        await driver.get('http://www.baidu.com');
        //name()表示找到页面中input的name值
        //sendKeys的第一个值表示自动化在文本框中输入的内容
        //第二个值Key.RETURN  表示按回车
        await driver.findElement(By.name('q')).sendKeys('Hello', Key.RETURN);
        //匹配自动化搜索后出现的数据
        //数字1000就是测试需要等待的时间，如果网慢，可以调大一些。
        await driver.wait(until.titleIs('Hello_百度搜索'), 1000);
    } finally {
        //系统要退出
        await driver.quit();
    }
})();
```
特别注意的是最后driver.quit()，系统一定要退出，如果不退出的话，就会堵塞后面的加载。

###启动测试

selenium-webdriver是基于node的语法，因此执行

```
node ./e2e/*.spec.js
```
就可以启动测试

nightwatch也是做e2e测试的一个框架，这里先不说，以后用这个做e2e。

###用rize来做e2e测试

[rize.js](https://rize.js.org/zh-CN/)是一个新出的测试框架，在这里检测一下e2e测试。 与rize.js配合的是一个新出的无头浏览器puppeteer，它和selenium-webdriver不同的是它不需要打开浏览器去测试。

rize.js实际上是对puppeteer的一个高度封装

###安装rize和puppeteer

```
npm install –save-dev puppeteer rize(必须科学上网装)
```
###测试用例

和selenium-webdriver一样，我们假设测试giuhub网站，输入node 会不会出现Node.js这几个字。（注意的是rize搜索是全页面的搜索，因此会慢些）。 在e2e文件夹下创建github.spec.js

```
const Rize = require("rize");
const rize = new Rize();
rize
    .goto('https://github.com/')        //打开url
    .type('input.header-search-input', 'node')  //找到文本框 输入文本
    .press('Enter')        //进行回车
    .waitForNavigation()    //等待
    .assertSee('Node.js')   
```
可见上述用例在语法上就比较简洁，容易看懂。

###启动测试

rize是基于node的语法，因此执行
```
node ./e2e/*.spec.js
```