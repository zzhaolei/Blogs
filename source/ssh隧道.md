---
title: ssh隧道
copyright: true
date: 2018-08-06 16:34:10
tags:
categories:
---

# ssh隧道

有时我们想访问一个机器`10.1.2.3`，但是这个机器我们无法直接访问，而`10.2.3.4`可以访问`10.1.2.3`，这时我们就可以使用`SSH隧道`，通过`10.2.3.4`跳转到`10.1.2.3`。

命令：
```
# ssh 命令/参数 本地主机(127.0.0.1)的端口:远程主机(10.1.2.3)的端口 用户@跳转主机(10.2.3.4)的IP
ssh -NTf -R 9200:127.0.0.1:1888 root@192.168.0.9
```

相关参数的解释：
```
-f Fork into background after authentication.
后台认证用户/密码，通常和-N连用，不用登录到远程主机。

-L port:localhost:localhost_port
将本地机(客户机)的某个端口转发到远端指定机器的指定端口. 工作原理是这样的, 本地机器上分配了一个 socket 侦听 port 端口, 一旦这个端口上有了连接, 该连接就经过安全通道转发出去, 同时远程主机和 localhost 的localhost_port端口建立连接. 可以在配置文件中指定端口的转发. 只有root才能转发特权端口. IPv6 地址用另一种格式说明: port/host/localhost_port

-R port:localhost:localhost_port
将远程主机(服务器)的某个端口转发到本地端指定机器的指定端口. 工作原理是这样的, 远程主机上分配了一个 socket 侦听 port 端口, 一旦这个端口上有了连接, 该连接就经过安全通道转向出去, 同时本地主机和 host 的 localhost_port 端口建立连接. 可以在配置文件中指定端口的转发. 只有用 root 登录远程主机才能转发特权端口. IPv6 地址用另一种格式说明: port/host/localhost_port

-D port
指定一个本地机器 “动态的” 应用程序端口转发. 工作原理是这样的, 本地机器上分配了一个 socket 侦听 port 端口, 一旦这个端口上有了连接, 该连接就经过安全通道转发出去, 根据应用程序的协议可以判断出远程主机将和哪里连接. 目前支持 SOCKS4 协议, 将充当 SOCKS4 服务器. 只有 root 才能转发特权端口. 可以在配置文件中指定动态端口的转发.

-C Enable compression.
压缩数据传输。

-N Do not execute a shell or command.
不执行脚本或命令，通常与-f连用。

-g Allow remote hosts to connect to forwarded ports.
在-L/-R/-D参数中，允许远程主机连接到建立的转发的端口，如果不加这个参数，只允许本地主机建立连接。注：这个参数我在实践中似乎始终不起作用。
```
