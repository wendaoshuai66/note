#Linux补充
## 认识 Linux 环境

![网卡的配置文件](https://wendaoshuai66.github.io/study/note/images/Linux.png)
##终端软件
###老牌的终端软件 putty

###[XShell](https://xshell.en.softonic.com/)
### [Cmder](https://cmder.net/) (比较推荐在微软 Terminal出之前) 

###可以使用该终端软件 ssh远程到服务器上,例如：
```
ssh root@192.168.0.104
```
ssh远程登录到服务器，退出

```
exit
```
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



##查看ip
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
