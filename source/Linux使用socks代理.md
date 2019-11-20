---
title: Linux使用socks代理
copyright: true
date: 2018-09-25 11:27:16
tags:
categories:
---

# Linux使用socks代理

## 安装一些依赖
```
yum install epel-release
yum install python-pip python-devel gcc gcc-c++ cmake git
```

## 安装`shadowsocks`

`pip install shadowsocks`, 编辑文件(没有就新建), `vim /etc/shadowsocks.json`, 添加以下内容:
```
{
    "server": "192.168.0.1",
    "server_port": 34523,
    "local_address": "127.0.0.1",
    "local_port": 1080,
    "password": "password",
    "timeout": 300,
    "method": "aes-256-cfb",
    "fast_open": false
}
```

 - server代表shadowsocks服务器IP
 - server_port是服务器端口
 - local_address是本地IP
 - local_port为本地端口(默认为1080)
 - password是shadowsocks账号的密码
 - timeout是超时时间
 - method为加密方式
 - fast_open默认设置为false

## 启动和停止
```
# 启动
sslocal -c /etc/shadowsocks.json -d start
# 停止
sslocal -c /etc/shadowsocks.json -d stop
```

## Proxychains4
有些终端里面只支持`http`和`https`，而`shadowsocks`使用的是`socks`，使用`Proxychains4`将`socks`转为`http`。
```
git clone https://github.com/rofl0r/proxychains-ng.git
cd proxychains-ng
./configure
make && make install
cp ./src/proxychains.conf /etc/proxychains.conf
cd .. && rm -rf proxychains-ng
```

编辑`vim /etc/proxychains.conf`，将`socks4 127.0.0.1 9095`改为`socks5 127.0.0.1 1080`，然后直接使用`proxychains4 + 命令`的方式，例如`proxychains4 curl ip.cn`查看ip地址。
