#HTPP协议那些事

##HTTP请求模型

![HTTP请求模型](https://wendaoshuai66.github.io/study/note/images/http1.jpg)

###两个角色 两个动作

客户端：动作的发起者(通过HTTP协议去向服务端请求资源)
服务端：动作的响应者(通过HTTP协议去向客户端响应资源)

###区分客户端和服务端
谁发请求谁是客户端，谁响应请求谁是服务端。

有时候很多软件会把客户端和服务端做在一起：比如nodejs，nodejs向前端渲染页面的时候是作为服务端；向真正的后端(java)去请求数据的时候作为客户端。

##浏览器行为与HTTP协议

![HTTP请求模型](https://wendaoshuai66.github.io/study/note/images/http2.jpg)

###处理流程

1.输入网址并回车
  准备工作：测试网络的连通性，要保证能够连上互联网。(测试从内网跨越到外网去，这个障碍可能是路由器、网关等等)
  
Linux防火墙是是 FireWall 更久以前是 iptables

2.解析域名

 输入的网址是一段字符串，必须通过DNS服务器做一下DNS域名解析。(DNS就是一个大型数据库，在这个数据库中就维护着域名和ip地址的映射关系)
 
3.浏览器发送HTTP请求

   Request：向服务器发送请求(通过5层协议模型，路由策略(路由转发算法))
   
4.服务器处理请求

  通过nginx反向代理找到对应的服务器，然后进行服务器处理。
  
5.服务端返回HTML响应

 Response：向客户端响应请求
 
6.浏览器处理HTML页面

  浏览器渲染页面
  
7.继续请求其他资源

  浏览器会对ip进行缓存，之后的请求是从第4步开始，一直请求响应直到把所有要请求的数据全部请求过来为止。

  
注：请求和响应的路由路径是运营商处理的。DNS不全是运营商处理，还有第三方的处理(比如说阿里)

```
第二幅图中一堵墙，可以是防火墙是网关，或者是家用路由器,外面是互联网，里面是局域网
第三幅图拿着第一幅图的地址，其实就是一串字符串，去DNS服务器上（多栏|–|–|）解析成ip地址。ipv4 int
第四幅图透明设备，指的就是我们的路由器。 把我们的数据包转发出去
第五幅图顺着网线终于找到了谷歌的服务器
第六幅图把请求发给服务器
第七幅图通过路由把结果返回给用户
```

##多层协议模型

###OSI/IOS协议模型

OSI/IOS协议模型是国际标准模型，共分为七层

![暂无图片](https://wendaoshuai66.github.io/study/note/images/http3.jpg)

###TCP/IP协议模型

TCP/IP协议模型是五层协议模型，对于我们前端来说，下面两层并不是那么关注，也可以说程4层协议模型



1.五层：应用层、传输层、网络层、数据链路层、物理层

2.四层：应用层、传输层、网络层、网络接口层
xa
![暂无图片](https://wendaoshuai66.github.io/study/note/images/http2.png)

1.应用层

 为用户提供所需要的各种服务,例如:HTTP、FTP、DNS、SMTP等.
 
2.传输层

 为应用层实体提供端到端的通信功能,保证数据包的顺序传送及数 据的完整性。
 
 该层定义了两个主要的协议:传输控制协议(TCP)和用户数据报协议(UDP).
 
3.网络层

 主要解决主机与主机之间的通信问题。IP协议是网际互联层最重要的协议
 
 ping命令是用的网络层 . window: trace Linux与 MAC traceroute 也是网络层的
 
4.数据链路层

 封装成帧、透明传输、差错检测
 
5.物理层

在物理网线上传送比特流

###在TCP/IP协议栈中的位置

![暂无图片](https://wendaoshuai66.github.io/study/note/images/http1.png)

##什么是HTPP协议

![暂无图片](https://wendaoshuai66.github.io/study/note/images/http3.png)


HTTP超文本传输协议，本质就是一种规范，约束响应与请求的动作，被一个组织所制定，在该文档是[RFC文档](https://wendaoshuai66.github.io/study/note/rest/HTTP协议_最新RFC文档_中文版.pdf)，是官方的解释。

##HTTP工作过程

一次HTTP操作被称为一个事务，其过程可分为四步：

###首先理解事务的概念：事务的特点，如果有一个操作，这个操作要分为若干个步骤去完成，而且这个操作要严格遵守这个步骤顺序去操作，同时只要在其中一个步骤中操作失败，那么这次操作就会失败，只要符合这种特点的操作就叫事务

HTTP操作事务分为四步：

1.首先客户端与服务器需要建立连接(TCP连接)。只要单击某个超级连接，HTTP的工作开始。

2.建立连接后，客户端发送一个请求给服务器，请求方式的格式为：统一资源标识符(URL)、协议版本号，后面的MIME信息，包括请求修饰符、客户端信息和可能的内容。

3.服务器接收请求后，给予相应的响应信息，其格式为一个状态行，包括信息的协议版本号，一个成功或错误的代码，后面是MIME信息，包括服务器信息、实体信息和可能的内容。

4.客户端接收到服务器所返回的信息通过浏览器显示在用户的显示屏上，然后客户端与服务器断开连接


如果在以上过程中的某一步出现错误，那么产生错误的信息返回到客户端，有显示屏输出。对于用户来说，这些过程是由HTTP自己完成的。用户只要用鼠标点击，等待信息显示就可以了。

##请求与响应

HTTP请求组成：请求行、消息报头、请求正文

HTTP响应组成：状态行、消息报头、响应正文

请求行组成：以一个方法符号开头(GET、POST等等)，后面跟着请求的URI(资源路径)和协议的版本

例如：POST / URI HTTP/1.1

状态行组成：服务器HTTP协议的版本，服务器发回的响应状态代码和状态代码的文本描述。


例如：HTTP/1.1 200 OK

###实际情况下，请求行和响应行会被整合到General里面


资源路径URI：Request URL: https://www.baidu.com/


方法符号：Request Method: GET

协议版本：通常会省略

响应状态吗：Status Code: 200 OK


![暂无图片](https://wendaoshuai66.github.io/study/note/images/request.png)


![暂无图片](https://wendaoshuai66.github.io/study/note/images/response.png)

##请求头消息报头

###常用的请求报头

消息报头中都是一些列的键值对来表示的。


. 浏览器所能接受的资源类型


```
Accept: text/html,application/xhtml+xml;q=0.9,image/webp,image/apng,* / *;q=0.8
```

. 浏览器所接受的资源是否支持压缩用什么方式压缩(节省时间，节省带宽)


```
Accept-Encoding: gzip, deflate, br
```

. 浏览器默认语言是什么

```
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
```

.支持长连接(控制长连接和短连接)


```
Connection: keep-alive
```

. Host请求报头域主要用于指定被请求资源的Internet主机和端口号,它通常从HTTP-URL中提取出来的,发送请求时,该报头域是必需的。


```
Host: www.baidu.com
```


. 请求报头域允许客户端将它的操作系统、浏览器和其它属性告诉服务器。(根据用户类型判断是不是爬虫，可用于反爬虫)Agent

```
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1

```

. 请求来源，可用于检查是不是跨域，检查是不是爬虫

```
Referer: https://www.baidu.com/
```


##响应头消息报头


###常用的响应报头

.缓存相关

```
Cache-Control：private
```

```
Date：Sun, 27 Jan 2019 08:04:07 GMT
```


```
Expores：Sun, 27 Jan 2019 08:04:07 GMT
```

```

etag
```

. Location响应报头域用于重定向接受者到一个新的位置。Location响应 报头域常用在更换域名的时候。

.Server响应报头域包含了服务器用来处理请求的软件信息。与User- Agent请求报头域是相对应的.

```

Server: BWS/1.1

```


###实体报头

请求和响应消息都可以传送一个实体。一个实体由实体报头域和实体正文组成,但并不是说实体报头域和实体正文要在一起发送,可以只发送实体报头域。实体报头定义了关于实体正文(eg:有无实体正文)和请求所标识的资源的元信息。

###常用的实体报头

Content-Encoding实体报头域被用作媒体类型的修饰符,它的值指示了已经被应用到实体正文的 附加内容的编码,因而要获得Content-Type报头域中所引用的媒体类型,必须采用相应的解码机制。

Content-Language实体报头域描述了资源所用的自然语言。

Content-Length实体报头域用于指明实体正文的长度,以字节方式存储的十进制数字来表示。

Content-Type实体报头域用语指明发送给接收者的实体正文的媒体类型。

Last-Modified实体报头域用于指示资源的最后修改日期和时间。

Expires实体报头域给出响应过期的日期和时间。


##请求方法

GET：请求获取Request-URI所标识路径

POST：在Request-URI所标识的资源后附加新的数据

HEAD：请求获取由Request-URI所标识的资源的响应消息报头（可以用于判断资源有没有变化，查看响应头里面的缓存策略的时间有没有变）

PUT：请求服务器存储一个资源，并用Request-URI作为其标识 (用于RESTful接口)

DELETE：请求服务器删除不Request-URI所标识的资源

TRACE：请求服务器回送收到的请求信息，主要用于测试或判断

COUNECT：HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器

OPTIONS：请求查询服务器的性能，或者查询与资源相关的选项和需求


###GET方法和POST方法的区别

来自w3schools上面的回答 ：


GET在浏览器回退时是无害的，而POST会再次提交请求。

GET产生的URL地址可以被Bookmark，而POST不可以。

GET请求会被浏览器主动cache，而POST不会，除非手动设置。

GET请求只能进行url编码，而POST支持多种编码方式。

GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。

GET请求在URL中传送的参数是有长度限制的，而POST么有。

对参数的数据类型，GET只接受ASCII字符，而POST没有限制。

GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。

GET参数通过URL传递，POST放在Request body中。

更深层次的理解：

HTTP是基于TCP/IP的关于数据如何在万维网中如何通信的协议。因此GET和POST的根本是TCP

GET产生一个TCP数据包；POST产生两个TCP数据包。

    对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）

    对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200         
    ok（返回数据）
    
    
并不是发一次包就快，发两次包就慢。据研究，在网络环境好的情况下，发一次包的时间和发两次包的时间差别基本可以无视。而在网络环境差的情况下，两次包的TCP在验证数据包完整性上，有非常大的优点。


####并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次


##HTTP状态码


![HTTP状态码](https://wendaoshuai66.github.io/study/note/images/zhuangtai.png)


##cookie与session

HTTP是一个无状态协议：什么状态？是维持服务器与客户端连接通信的状态。


为了维持这样的状态，引入了session，session需要cookie配合。

###cookie

Cookies是保存在客户端的小段文本,随客户端点每一个请求发送该url下的所有cookies到服务器端。

![cookie](https://wendaoshuai66.github.io/study/note/images/cookie.png)

####与Cookie相关的HTTP扩展头

```
Cookie: 
        BAIDUID=E5CEF5D82F43CCE6E493882174E3B741:FG=1; 
        BIDUPSID=E5CEF5D82F43CCE6E493882174E3B741; 
        PSTM=1544774928; 
        BD_UPN=123353; 
        __cfduid=de8eb2f58580a76ba2732bfe5c805bf7b1546698346; 
        BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; 
        delPer=0; 
        BD_HOME=0; 
        BD_CK_SAM=1; 
        PSINO=2; 
        locale=zh; 
        H_PS_PSSID=1454_21111_18559_28329_22160; 
        BDSFRCVID=WdFsJeCCxG3Javo9Eee93qR0uQmm7Y5SS3lS3J; 
        H_BDCLCKID_SF=JJkO_D_atKvjDbTnMITHh-F-5fIX5-RLf5cRa-OF5lOTJh0RyxOrDTFkbfDf-pT7KCJD0tJLb4DaStJbLjbke6oWeH0ft6KsKKJyLR-8bn5_Hn5k5KTjhPr2bl5y2RJyb6r20C6jb-cjfTu4DT8BjT5HDG-Jq6-qHJue_II5JDvRehQRy4oTLnk1DP6K-t483CT-VCOhLDJhVJ6zjpuh0pJyhb-eBjT2-DA_oCDbfCTP; 
        BDRCVFR[tox4WRQ4-Km]=mk3SLVN4HKm; 
        BDRCVFR[-pGxjrCMryR]=mk3SLVN4HKm; 
        BDRCVFR[CLK3Lyfkr9D]=mk3SLVN4HKm

```


Cookie 中最重要的是ID，开始的时候客户端是没有这个ID的，当第一次请求的时候服务器会给客户端分配ID，这个ID相当于凭据(VIP)以后再来请求的时候，客户端带这个ID,服务器看到这个ID，就知道曾经来请求，服务器就是在session中记录。

####设置cookie

请求头Cookie：客户端将服务端设置的Cookie返回到服务器

响应头Set-Cookie：服务器向客户端设置Cookie

服务器在响应消息中用Set-Cookie头将Cookie的内容回送给客户端，客户端在新的请求中将相同的内容携带在Cookie头中发送给服务器从而实现会话的保持。

```
Set-Cookie: delPer=0; path=/; domain=.baidu.com
Set-Cookie: BDSVRTM=15; path=/
Set-Cookie: BD_HOME=0; path=/
Set-Cookie: H_PS_PSSID=1454_21111_18559_28329_22160; path=/; domain=.baidu.com  记录的数据，可以自己设置
```

###session

session就是一个小数据库，记录这给客户端的编号，它的身份和其他的信息。

session永远是存在服务器端的。session中的sessionID服务器维护，服务器根据session判断用户。


![session](https://wendaoshuai66.github.io/study/note/images/cookie.png)

###session与cookie的区别和特点

cookie是浏览器这边的一个小数据库，session是服务器这边的一个小数据库

cookie是临时的，session也是临时的。

