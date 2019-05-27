#Linux补充
## 认识 Linux 环境

![网卡的配置文件](https://wendaoshuai66.github.io/study/note/images/Linux.png)
##远程登录Linux系统

一般的Linux系统都是用来做服务器来使用的，所以我们必须要操作它，服务器之所以叫做服务器是因为上面跑着很多服务，服务永不停止，这样才能在有需要的时候提供一些功能。如果不能远程登录到服务器上去，那么程序就没法部署上去。因为现在的服务器大多数都在机房或者都是云服务器，所以需要远程连接登录上去。

###远程登录协议
```
ssh root@192.168.0.104
```
ssh远程登录到服务器，退出

```
exit
```

###不同操作系统下远程登录的方式

windows

1 老牌的终端软件 putty

2.[XShell](https://xshell.en.softonic.com/)

3.[Cmder](https://cmder.net/) (比较推荐在微软 Terminal出之前) 
Linux和MacOS

Linux和MacOS的终端里面登录都自带了ssh命令，可以通过ssh命令来登录远程服务器。

###scp

scp是Linux上的一个命令，Mac也可以使用。这个命令的主要作用就是远程复制文件。

这个命令使用的是加密连接，和ssh加密的通道一样。


远程复制文件的三种姿势


1.本地文件复制到远程服务器上

把本地文件复制到远程服务器上是部署的一个基本操作，通过scp命令可以完成：

```
//scp 文件名 服务器的用户名@服务器的ip:复制到的路径
//如果上传的是目录: scp -r 文件名 服务器的用户名@服务器的ip:复制到的路径
scp test.zip root@192.168.0.104:/test//上面命令的意思是把本地的test.zip文件复制到远程
//服务器的/test目录去。
```


2.远程服务器的文件复制到本地

scp是一个远程复制文件命令，当然也可以从远程复制到本地。复制到本地很简单：

```
//scp 服务器的用户名@服务器的ip:文件路径 复制到的路径

//如果上传的是目录: scp -r 服务器的用户名@服务器的ip:文件路径 复制到的路径


 scp -r root@192.168.0.104:~/test ~/
```



3.远程服务器文件复制到远程服务器

这种方式指的是一个远程服务器的文件复制到另一个远程服务器上，而且只需要在本地执行。可想而知这个命令的强大。

```

//scp 服务器的用户名@服务器的ip:文件路径 服务器的用户名@服务器的ip:复制到的路径

//scp -r 服务器的用户名@服务器的ip:文件路径 服务器的用户名@服务器的ip:复制到的路径

scp -r root@192.168.0.104:~/test root@192.168.0.106:~/
```



help

更多的scp命令语法可以通过查看帮助文档来学习。

但是scp –help命令提供的是简单文档



##网络管理命令

###查看ip
```
ifconfig //非常古老的

ip addr//新版本（centos 7版本以上）

```
###有时候ip地址找不到的启动网卡

如果有时候我们要重启网卡，一定要在图形化界面上重启，如果在终端中远程登录，就是直接掉线。

网卡的配置文件：/etc/sysconfig/network-scripts/ifcfg- [name]
查看网卡的配置文件 cat /etc/sysconfig/network-scripts/ifcfg- [name] 如图：


![网卡的配置文件](https://wendaoshuai66.github.io/study/note/images/onboot.png)

安装的过程没有激活网卡 ONBOOT = “no”需要vi修改

关闭网卡：ifdown 网卡名称

启动网卡：ifup 网卡名称

###route

若果 route 没有找到，可以一下两种方式解决

第一种方式

```
//因为都是以前的命令所以找不到，可以安装
yum install net-tools
```
第二种方式

```
ip route
```

route命令用于帮助诊断网络。当局域网访问不了互联网的时候，我们首先使用ping命令执行下，如果没有得到返回，那么可能就是网络出了问题，那么就可以使用route命令查看一下路由配置对不对。

![route](https://wendaoshuai66.github.io/study/note/images/route.png)



Destination：路由名称。

Gateway：表示网关。

Genmask：表示通过网关可以达到什么样的网段。

Ifce：Interface，表示网络走的是什么网卡。



###查询Linux网络端口

####ss命令


```
ss -an //输出进程的信息就会包括端口号
```
如图

![ss](https://wendaoshuai66.github.io/study/note/images/ss.png)


```
ss-anp //显示使用网络的进程
```

如图

![ss-anp](https://wendaoshuai66.github.io/study/note/images/ssAnp.png)

使用管道机制查询单个网络端口

```
ss -anp | grep 80
ss -an | grep 80

```


#### netstat命令

netstat命令和ss命令用法相同。


##Linux进程管理相关命令

###top命令

top命令相当于windows的任务管理器，显示所有进程的状态。属于动态


![top](https://wendaoshuai66.github.io/study/note/images/top.png)

###ps命令

ps命令用于显示当前进程 (process) 的状态,查看当前操作系统某一瞬间净化才能的状态.

![ps](https://wendaoshuai66.github.io/study/note/images/ps.png)

```
ps 的参数非常多, 在此仅列出几个常用的参数并大略介绍含义
-A 列出所有的行程
-w 显示加宽可以显示较多的资讯
-au 显示较详细的资讯
-aux 显示所有包含其他使用者的行程
au(x) 输出格式 :
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
USER: 行程拥有者
PID: pid
%CPU: 占用的 CPU 使用率
%MEM: 占用的记忆体使用率
VSZ: 占用的虚拟记忆体大小
RSS: 占用的记忆体大小
TTY: 终端的次要装置号码 (minor device number of tty)
STAT: 该行程的状态:
D: 无法中断的休眠状态 (通常 IO 的进程)
R: 正在执行中
S: 静止状态
T: 暂停执行
Z: 不存在但暂时无法消除
W: 没有足够的记忆体分页可分配
<: 高优先序的行程
N: 低优先序的行程
L: 有记忆体分页分配并锁在记忆体内 (实时系统或捱A I/O)
START: 行程开始时间
TIME: 执行的时间
COMMAND:所执行的指令
```

#### ps aux

如图：

![ps aux](https://wendaoshuai66.github.io/study/note/images/psAux.png)


1.user：表示这个进程是以哪一个用户启动的。

2.command：进程的权限，是在用户身上继承下来的，如果以root身份启动，那么这个进程会获得root用户的所有权限。

3.pid：进程编号

4.stat : 当前一瞬间，进程的状态。(S：休眠状态，Z：僵尸状态(父进程已经死掉了，子进程没有被操作系统及时的回收))

特殊的进程：1号进程是用来管理其他进程的，这个进程是杀不了的，1号进程是所有其他进程的父进程，如果父进程死了，子进程一块死掉。

####查找特定的进程

```
ps aux | grep 服务名
```


管道命令 ‘|’ 符号： 把前面命令产生的结果，传给后面的命令

1.ps aux ：查看当前操作系统某一个瞬间所有进程的状态。

2.grep 服务名：筛选，在上面命令的结果中筛选出指定服务名的进程

![查找特定的进程](https://wendaoshuai66.github.io/study/note/images/psGrep.png)

显示了三个服务，但是只有上面两个是sshd的服务，第一个是父进程，第二个是子进程，第三个是grep查找的服务。


#####区分父进程和子进程

看pid，谁的pid小，谁就有可能是父进程。

###杀死进程

####kill命令

```
kill 进程的pid

kill -9 进程的pid //强制杀死进程
```

原理： kill功能是向操作系统发一个信号过去，linux操作系统有32种信号(各有各的用途)，-9 就表示9号信号，9号信号的意思就是强制关闭一个进程。不加 -9就是让进程自行退出的信号。

####pkill命令

pkill命令和kill类似，主要是pkill后面跟的是进程的名称，kill后面跟的是进程的pid。

```
kill 进程的名称
```


###w命令
显示当前谁登陆了远程服务器上。

![w](https://wendaoshuai66.github.io/study/note/images/w.png)

###last命令
账号所有的登陆历史
```
last
```

显示上10条

```
last -n 10
```
登陆失败的

```
lastb
```



##Linux怎么管理服务

###查看服务名

```
systemctl status 服务名 
```


###停止服务


```
systemctl stop 服务名 
```

###开启服务

```
systemctl start 服务名 
```


###重启服务

```
systemctl restart 服务名 
```


###重新加载一个服务的配置文件(不是通用的 nginx)

```
systemctl reload 服务名 
```
##操作系统概述

###古老的操作系统

古老的Dos系统，它是windows系统的前身，最早的windows是构建在Dos系统下面的，作为Dos系统的一种增强型界面。后来windows95出现之后，windows就独立了，它不依赖Dos了，反而Dos就成为了一个子系统嵌入到windows中，一直到今天(现在的终端不是Dos了，但是一直使用着Dos的命令)。



###windows

windows作为中国普遍的操作系统是适用于工作和娱乐的。但是在开发环境下相对于Mac和Linux就有些欠缺，windows的终端也没有Mac和Linux强大。

###Linux

Linux系统是一个开源的操作系统，适合程序员做开发。Linux的桌面不是很成熟，但是服务器系统是非常强大的。

###MacOS

MacOS应该是程序员中最普遍使用的操作系统了，有着成熟的桌面同时终端也非常强大，适合程序员做开发

##行编辑器vi/vim

由于我们是远程登录到Linux服务器，又因为远程服务器并没有图形化界面，所以在编辑器方面vim是一个神奇。vim是vi的增强版。

![vim](https://wendaoshuai66.github.io/study/note/images/vi-vim-cheat-sheet-sch.gif)

1.vim三种模式：命令模式、插入模式、编辑模式。使用ESC或i或：来切换模式。

2.命令模式下

```

:q  退出

:q! 强制退出

:wq 保存并退出

:set number  显示行号

:set nonumber  隐藏行号

/host  在文档中查找host 按n跳到查找下一个，shift+n查找上一个

yyp  复制光标所在行，并粘贴

h(左移一个字符←)、j(下一行↓)、k(上一行↑)、l(右移一个字符→)
```

##下载命令

在终端中的下载命令有两个。

1.wget

```
wget url 下载

wget -c url ：断点续传，下载失败了，执行这个会在断开的地方下载。
```

2.curl

```
curl url 下载
```


curl功能更加强大，可以控制http头


##Linux帮助

1.命令 -h：显示帮助。

2.man 命令：显示命令的帮助文档。这个man命令实际上是一个小数据库，存放着各种命令的帮助文档。

##常用的终端快捷键

ctrl + s：暂停屏幕输出：不要轻易的挂起终端，可能会导致一个服务启动失败。

ctrl + c：结束正在运行的程序(ping,telnet等)

ctrl + d：结束输入或退出shell

ctrl + q：恢复屏幕输出。

ctrl + l：清屏，等同于Clear。

ctrl + a/ctrl + e 快速移动光标到行首/行尾。

