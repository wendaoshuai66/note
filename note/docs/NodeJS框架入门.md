#NodeJs 框架入门


##什么是Exress

Express是一个简洁而灵活的nodejs Web应用框架，提供了一系列强大特性帮助你创建各种Web应用。


##Express框架的特性

可以设置中间件来响应http请求

定义了路由表用于执行不同的http请求动作

可以通过向模板传递参数来动态渲染HTML页面

##什么是中间件

上面Exress框架的特性第一条就提到了中间件，中间件是贯穿Expres全程的一个东西，到底什么是中间件。

#####所谓中间件就是：从客户端请求的一端到服务器响应客户端的一端中间所使用到的插件就叫做中间件。

###中间件的功能

1.执行任何代码

2.修改请求和响应对象

3.终结请求-响应循环。

4.调用堆栈的下一个中间件 如果当前中间件没有终结请求响应循环(res.render,res.send,res.json等)，则必须调用next()方法把控制权交给下一个中间件。如果终止了的话，下一个中间件渲染会报错，但是node代码正常执行。


###Express应用中间件的类型

应用级中间件

路由级中间件

错误处理中间件

内置中间件

第三方中间件

###应用级中间件

应用级中间件绑定到app对象使用app.use()和app.METHOD方法(HTTP方法)

```
const app = express();
    //没有挂在路径的中间件，应用的所有请求都会执行该中间件
    app.use((req,res,next) => {
        console.log('必经路由')
        next();
    })
    //挂在路径的中间件，任何指向此路径的中间件都会执行它
    app.use('/user/:id',(req,res.next) => {
        console.log('/user/中间件')
        next();
    })
    //使用http方法的中间件
    app.get('/user/:id',(req,res,next) => {
        res.send('http方法中间件')
    })
```

###装载组中间件

装载组中间件就是对一个挂载点挂载一组中间件。

```
app.use(/user/:id,(req,res,next) => {
    console.log('one');
    next();
},(req,res,next) => {
    console.log('two');
    next();
})
```
如果在中间件栈中跳过剩余的中间件，可以使用next(‘route’)方法将控制权交给下一个路由。

#####注意：next(‘route’)只对使用app声明的和router声明的中间件有效。

###路由级中间件

路由级中间件和应用级中间件一样，只是它绑定的对象为express.Router();

```
const router = express.Router()
```


router中间件和app中间件处理的情况一样，只是router中没有app中特别复杂的api，只有路由。参照应用级中间件的使用方法。

###错误处理中间件


错误处理中间件有4个参数，定义错误处理中间件时必须使用这4个参数，即使不需要next对象，也必须在方法签名中声明它，否则把会错误处理中间件当做常规中间件处理。错误中间件容错要放在所有中间件后面。

```
app.use((err,req,res,next) => {
    console.log(err.stack);
    res.status(500).send('服务端出错');
})
```

###内置中间件
express.static是Express唯一内置的中间件，它基于server-static,负责在Express中托管静态资源.

```
 app.use(express.static('public'));
```
###第三方中间件

安装第三方中间件，通过使用app.use的方式把第三方中间件加载到应用里面，可以在应用级加载，也可以在路由级加载。

比如cookie-parse：用于解析cookie,如果不使用这个，就拿不到cookie



body-parser nodejs中间件，用于处理JSON、Raw、Text和URL编码的数据

```
npm install body-parse –save
```

cookie-parser 这就是一个解析Cookie的工具，通过req.cookies可以取到传过来的cookie，并把他们转成对象

```
npm install cookie-parser –save
```

multer nodejs中间件，用于处理enctype=‘multipart/form-data’(设置表单的MIME编码)的表单数据

```
npm install multer –save

```

###如何开发一个第三方中间件

由上面总结可得知，一个中间件就是一个函数，而第三方中间件同样也是一个全局的应用级中间件。通过这些信息就可以写出一个中间件。

例如：开发一个事件中间件，在req上绑定使用事件

```
const timeMiddleware() => (req,res,next) => {
    req.requestTime = Date.now();
    //更复杂的处理逻辑
    next();
}
module.exports = timeMiddleware;
```

引入和使用

```
const timeMiddle = require('timeMiddleware');
app.use(timeMiddle());
app.get('/',(req,res) => {
    console.log(req.requestTime);
})
```

这样一个简单的中间件就完成了。

##使用Express

###安装experss

```
npm install express –save
```

#####注意：项目名不要和包名相同，否则无法安装依赖包

###创建app.js

在项目根目录创建app.js ，app.js是express框架的启动文件。

###在app.js中引入express模块

```
const express = require('express');
```

###通过express实例化express对象

```
const app = express();
//相当于
const app = new express();
```

###app对象的一些常用方法

app.use 通过app.use 引入中间件

app.route 注册路由

app.listen 创建web服务

app.METHOD(METHOD为http方法)拿到当前用户的路由,渲染结果。

app.set 设置所需要的模板

app.engine 设置模板用什么模板引擎编译

###创建一个express Web服务

在app.js中书写一下代码。

```
const express = require('express');
const app = express();

app.get('/',(req,res)=>{
    res.send('hello express')
})

app.listen('3000',()=>{
    console.log('启动成功')
})
```

###启动服务

在项目根目录终端使用下面命令

```
node app.js
```

至此一个web服务就启动了，但是问题来了，当我们修改代码的时候，页面并不会修改，只能手动停掉服务，然后再启动才会生效。所以要开启热启动

###开启热启动

热启动的方法有两种：一种是supervisor,一种是nodemon

1.supervisor

安装
```
npm install supervisor -g
```

启动


```
supervisor app.js
```

2.nodemon

安装

```
npm install nodemon -g
```

启动

```
nodemon app.js
```

###用户的请求与响应对象

在开启服务的使用，每一个对应的请求里面都会有请求对象和响应对象。

####request：用户发出的请求

req.query 拿到所有的用户请求参数(get请求)

req.body 当使用post请求的使用，使用中间件获得参数，参数会在req.body里面。

req.params 拿到对应的路由参数

req.app 当callback为外部文件时，用req.app访问express的实例

req.baseUrl 获取路由当前安装的URL路径

req.hostname 获取主机名

req.ip 获取IP地址

req.route 获取当前匹配的路由

req.path 获取请求路径

req.subdomains 获取子域名

req.get() 获取指定的HTTP请求头

req.is() 判断请求头Content-Type的MIME类型

req.protocol 获取协议类型

req.cookies 获得 Cookies

####response：服务端向用户输出的东西
res.download() 提示下载文件

res.end() 终结响应处理流程

res.json() 发送一个JSON格式的响应

res.jsonp() 发送和一个支持JSONP格式的响应

res.redirect() 重定向请求

res.render() 渲染视图模板

res.send() 发送各种类型的数据

res.sendFile() 以八字节流的形式发送文件

res.sendStatus() 设置响应状态码，并将其以字符串的形式作为响应体一部分发出去

res.cookie(name，value [，option]) 设置Cookie

res.set() 设置请求头

res.append() 追加指定HTTP头

res.clearCookie() 清除Cookie

res.type() 设置Content-Type的MIME类型

###路由

所谓路由就是通过访问不同的url路径的得到不同的信息。

####早期的伪静态路由


在路由上写/index.html，并不会访问到这个页面，代码中还是动态路由

```
app.get('/index.html',(req,res) => {
    console.log(req.query);
    res.send('Hello ' +  req.query.username);
})
```

####路由参数

在路由后加:id,这个id就是路由参数,通过req.params.id拿到这个参数

```
app.get('/index/:id',(req,res) => {
    console.log(req.params.id);
})
```

####RESTful路由

RESTful 的核心思想就是，客户端发出的数据操作指令都是”动词 + 宾语”的结构。比如，GET /articles这个命令，GET是动词，/articles是宾语。[阮一峰的RESTful实战](http://www.ruanyifeng.com/blog/2018/10/restful-api-best-practices.html)

###引入静态文件

通过静态资源的方式引入一个静态资源文件夹public

```
app.use(express.static('public'))
```

访问静态文件 public目录就是根目录

#####例如：
在public目录下创建index.css文件，启动服务访问http://127.0.0.1:8080/index.css 就可以访问到。

###渲染html

通过sendFile的方式渲染出html，在项目根目录下创建views文件夹用来存放渲染视图，在views中常见index.html。在app.js中书写一下代码。

```
app.get('/index',(req,res) => {
    res.sendFile(__dirname + '/views'+ req.url + '.html');
})

```

通过访问http://127.0.0.1:8080/index 就可以得到

###通过body-parser取到post方式请求的数据

安装body-parser包

```
npm install body-parser –save

```

引入body-parser包

```
const bodyParser = require(‘body-parser’);
```

使用中间件

```
app.use(bodyParser.urlencoded({extended:false}))
```

取到参数

```
请求中使用req.body取得请求参数列表
```

##express路由的使用

###什么是路由

路由是指如何定义应用的端点以及如何响应客户端的请求

我们所谓的Controller就是路由控制器

###路由的组成部分

路由是由url、http请求(GET，post)和若干句柄组成。

路由结构如下：app.METHOD(path,{callback..},callback)

1.app是express对象的一个实例

2.METHOD是一个HTTP请求方法

3.path是服务器上的路径

4.callback是当路由匹配时要执行的函数。

###路由方法

get、post、put、delete代表CRUD的增删该查

get方法

```
app.get('/',(req,res) => {
    res.send('GET 请求')
});
```

post方法

```
app.post('/',(req,res) => {
    res.send('POST 请求')
});
```

put方法

```
app.put('/',(req,res) => {
    res.send('PUT 请求')
});
```

delete方法

```
app.delete('/',(req,res) => {
    res.send('DELETE 请求')
});

```

###标准的路由格式

标准的路由控制格式为：controller/action 这样的形式，一个控制器对应多个操作ID。

###app.all方法

app.all是一个特殊的路由方法，没有任何HTTP方法与其对象，无论是什么HTTP请求都会命中这个路由方法。

```
app.all((req,res,next) => {
    res.send('hello')       //无论是什么HTTP请求都会命中
    next(); 把当前的这个请求交给下一个请求处理函数
})
```

###路由路径

路由路径与请求方法结合，定义可以进行请求的端点。路由路径可以是字符串、字符串模式和正则表达。

###多个路由处理函数

请求处理多个回调函数，使用next()方法传递请求,如果应用已经响应了用户，后面的响应就不会进行了，node代码可以继续执行。

方式如下：

```
app.get('/index',(req,res,next) => {
    console.log(1);
    next();
},(req,res,next) =>{
    console.log(console.log(req));
})

```

###使用回调函数数组处理路由

```

const cb0 = (req,res,next) => {
    console.log('cb0');
    next();
}
const cb1 = (req,res, next) => {
    console.log('cb1');
    next();
}
const cb2 = (req,res,next) => {
    res.send('fdsaf');
}
app.get('/index',[cb0,cb1,cb2]);

```

###使用混合函数和函数数组处理路由

```
const cb0 = (req,res,next) => {
    console.log('cb0');
    next();
}
const cb1 = (req,res, next) => {
    console.log('cb1');
    next();
}
app.get('/index/b',[cb0,cb1],(req,res,next) => {
    console.log('hello');
    next();
},(res,req,next) => {
    res.send('wang');
})
```

###使用app.route()创建路由

app.route方法可以为一个路由路径建立多种请求方法

```

app.route('/index')
    .get((req,res) => {
        console.log('get');
    })
    .post((req,res) => {
        console.log('post');
    })
    .put((req,res)=> {
        console.log('put');
    })
```

###使用express.Router类创建模块化，可挂在的路由句柄

Router实例是一个完整的中间件和路由系统


使用

```
const express =require('express');
const router = express.Router();
```

使用router定义全局路由(必经)

定义在所有路由的最顶端

```
router.use((req,res,next) => {
    console.log('必经路由');
    next();
})
二者相同
app.use((req,res,next) => {
    console.log('必经路由')
    next();
})
```

这相当于定义了一个过滤的路由，所以路由句柄都会经过这个路由进行过滤,和*相同。


在应用中加载路由模块

```
 const router = require('./router');
app.use('/router',router);
```

##express错误处理及log4js日志

###express的错误处理

错误处理是express的全局一个中间件，错误处理中间件值定义在所有中间件的最下面的。它的工作流程是，只要有错误处理，有错误的中间件就会next到错误处理中间件，否则就不会执行到它。执行到错误处理中间件之后web实例又被重新唤醒继续执行node代码。

###错误处理中间件

express错误处理中间件有着严格的方法签名：必须要有4个参数，即便是用不到也要写上。

```
app.use((err,req,res,next) => {
    console.log(err);
})
```

err是错误对象
req是请求对象
res是响应对象
next是进行下一个中间件的操作


###两个常用的错误处理中间件

1.404错误

```
'use strict';
module.exports = function(template){
    return function fileNotFound(req,res,next) {
        var model = {url : req.url,statusCode : 404};
        if(req.xhr){
            res.send(404,model);
        }else {
            res.status(404);
            渲染出错误处理的模板
            res.render(template,req,data);
        }
    }
}
```



2.500 错误

```
var http = requier('http');
module.exports = function(template){
    return function serverError(err,req,res,next) {
        if(!res.statusCode || res.status != 200){
            res.statusCode = 500;
        }
        var desc = http.STATUS_CORES[res.statusCode];
        res.end(desc + '\n' | err);
    }
}

```

###日志系统log4js

log4js是第三方日志系统。

###安装

```
npm install log4js –save
```


###使用

```
const log4js = require('log4js');
log4js.configure({
    appenders : {
        log : {     //日志名字   可以写多个
            type : 'file',      //日志类型
            filename : './logs/log.log'     //日志存放路径
        }
    },
    categories : {
        default : {
            appenders : ['log']     //日志名字放在这
            level : 'info'      //日志等级，只会打印相同等级或者比此等级低的日志
        }
    }
})
var logger = log4js.getLogger('log');   //获取到此日志
app.use(log4js.connectLogger(logger));  //载入中间件
```

###日志等级区分

```
[2019-01-25T13:45:58.890] [INFO] log - ::ffff:127.0.0.1 - - "GET /err HTTP/1.1" 404 142 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"
[2019-01-25T13:46:02.370] [INFO] log - ::ffff:127.0.0.1 - - "GET / HTTP/1.1" 304 - "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"
[2019-01-25T13:47:16.910] [INFO] log - ::ffff:127.0.0.1 - - "GET /test HTTP/1.1" 500 1582 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"
[2019-01-25T13:48:39.321] [INFO] log - ::ffff:127.0.0.1 - - "GET /test HTTP/1.1" 500 1582 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"
[2019-01-25T13:48:40.037] [INFO] log - ::ffff:127.0.0.1 - - "GET /test HTTP/1.1" 500 1582 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"
[2019-01-25T13:48:40.220] [INFO] log - ::ffff:127.0.0.1 - - "GET /test HTTP/1.1" 500 1582 "" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36"

```

###封装日志中间件

```
const log4js = require('log4js');
module.exports = (type='file',path='./log.log',level='info')=> {
    log4js.configure({
        appenders : {
            log : {     //日志名字   可以写多个
                type : type,      //日志类型
                filename : path     //日志存放路径
            }
        },
        categories : {
            default : {
                appenders : ['log']     //日志名字放在这
                level : level     //日志等级，只会打印相同等级或者比此等级低的日志
            }
        }
    })
var logger = log4js.getLogger('log');   //获取到此日志
    return log4js.connectLogger(logger);
}
```

##express模板引擎的使用

###什么是模板引擎

模板引擎就是使我们能够在应用程序中使用静态模板文件。在运行时，模板引擎用实际值替换模板文件中的变量，并将模板转换为发送到客户端的HTML文件。通俗的讲就是使用一个模板可以在里面写变量，写循环，然后通过服务端编译程html文件。

###swig.js 模板引擎

swig.js是一个html模板引擎，它支持.html后缀名。

###安装swig

```
npm install swig –save
```

###引入swig

```
const swig = require('swig');

```

###设置模板引擎为.html

```
app.set('view engine','html');
```

###设置让swig来编译模板引擎

```
app.engine('html',swig.renderFile);
```

###基础模板

在项目根目录创建layout文件夹存放基础模板，在其中创建基础的模板文件layout.html,基础就是通用的模板，可以让模板继承自基础模板，省去重复的代码。内容如下。

上面文件中{}包裹的地方叫做区块，这样的区块留给真正要实现的文件。这里只是作为一个基础模板来使用。

###创建模板

在项目中创建views文件夹，所有的模板引擎都放到这里面。在views文件夹下建立index.html 并继承自loyout.html，内容如下：

在html中的区块相当于往继承过来的模板中填充代码。title这样的数据是从服务端传过来的数据。

###服务端渲染

使用服务端渲染，渲染出模板，并且可以往模板中传递数据。

```
app.get('/test',(req,res) => {
    res.rendr('index',{
        title : '测试首页',
        data : 'Hello express'
    })
})
```

这时index.html就可以修改一下。

这样就可以取到值了。

#####注意：模板不要滥用，所用的数据都往前台输出，会造成模板和后台的压力很大，而把一些有必要的东西输出。

##KOA 

Koa -- 基于 Node.js 平台的下一代 web 开发框架

###简介

Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

###官网

学习可以看[官方文档](https://koa.bootcss.com/#)

