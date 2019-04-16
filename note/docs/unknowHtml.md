#你不知道的HTML
##什么是同源策略
是一种约定，它是浏览器最核心最基本的安全功能，1995年，同源政策由 Netscape 公司引入浏览器。目前，所有浏览器都实行这个政策。所谓"同源"指的是"三个相同"。
​                                                                   
    协议相同
    域名相同
    端口相同

##同源策略目的
同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。
## 非同源策略有三种行为限制

（1） Cookie、LocalStorage 和 IndexDB 无法读取。

（2） DOM 无法获得。

（3） AJAX 请求不能发送。
## 如何设置同源策略（hosts）
Cookie 是服务器写入浏览器的一小段信息，只有同源的网页才能共享。但是，两个网页一级域名相同，只是二级域名不同，浏览器允许通过设置document.domain共享 Cookie。
例如test1.xxx.com/a.html 与test2.xxx.com/b.html

在a.html中

```javascript 
document.domain = 'xxx.com'//设置同源策略
document.cookie = "test1=hello";
```

在b.html中可以读到Cookie

```javascript
var allCookie = document.cookie;
```

另外，服务器也可以在设置Cookie的时候，指定Cookie的所属域名为一级域名，比如`.example.com`。

``` javascript
Set-Cookie: key=value; domain=.example.com; path=/
```

​	                                                                                                                                                                                                                                   