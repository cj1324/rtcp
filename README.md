rtcp.py
=======

desc
----

利用python的socket端口转发，用于远程维护。
如果连接不到远程，会sleep 36s，最多尝试200(即两小时)。

usage
-----

./rtcp.py stream1 stream2

stream为：l:port或c:host:port

l:port表示监听指定的本地端口

c:host:port表示监听远程指定的端口


使用场景
--------

###一

A服务器在内网，公网无法直接访问这台服务器，但是A服务器可以联网访问公网的B服务器（假设IP为222.2.2.2）。

我们也可以访问公网的B服务器。我们的目标是访问A服务器的22端口。那么可以这样：

+ 在B服务器上运行：

> `./rtcp.py l:10001 l:10002`
>
> 如果有root权限的情况下，还可以选择监听对应的网口。提升安全性(root也有风险)
> `./rtcp.py l:10001:eth0 l:10002:lo`
>
>表示在本地监听了10001与10002两个端口，这样，这两个端口就可以互相传输数据了。

+ 在A服务器上运行：

> `./rtcp.py c:localhost:22 c:222.2.2.2:10001`
> 表示连接本地的22端口与B服务器的10001端口，这两个端口也可以互相传输数据了。

+  然后我们就可以这样来访问A服务器的22端口了：

> `ssh 222.2.2.2 -p 10002`
> 原理很简单，这个命令执行后，B服务器的10002端口接收到的任何数据都会传给10001端口，此时，A服务器是连接了B服务器的10001端口的，数据就会传给A服务器，最终进入A服务器的22端口。

###二

不用更多举例了，rtcp.py的l与c两个参数可以进行灵活的两两组合，多台服务器之间只要搞明白数据流方向，那么就能满足很多场景的需求。

贡献
----

@author: watercloud, zd, knownsec team

@web: www.knownsec.com, blog.knownsec.com
