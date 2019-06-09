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

###几个常用的中间件

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