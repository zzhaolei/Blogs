---
title: CentOS 7安装Rabbitmq
copyright: true
date: 2018-07-24 16:07:34
tags:
categories:
---

# 安装Rabbitmq

## 安装依赖
 - CentOS/Fedora:
    `yum install -y gcc gcc-c++ cmake ncurses-devel openssl-devel`

## 下载相关软件包
 - 安装`Erlang`
    `curl -o otp_src_21.0.tar.gz http://erlang.org/download/otp_src_21.0.tar.gz`

解压`tar -xzvf otp_src_21.0.tar.gz`

进入目录`cd otp_src_21.0`

预编译`./configure --prefix=/usr/local/erlang`

执行编译和安装`make && make install`

将执行文件链接到`/usr/bin`下:
```
ln -s /usr/local/erlang/lib/erlang/bin/ct_run /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/dialyzer /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/epmd /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/erl /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/erlc /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/escript /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/run_erl /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/to_erl /usr/bin/
ln -s /usr/local/erlang/lib/erlang/bin/typer /usr/bin/
```

## 安装Rabbitmq

下载通用`Unix`软件包, `curl -o rabbitmq-server-generic-unix-3.7.7.tar.xz https://dl.bintray.com/rabbitmq/all/rabbitmq-server/3.7.7/rabbitmq-server-generic-unix-3.7.7.tar.xz`

解压`tar -xzvf rabbitmq-server-generic-unix-3.7.7.tar.xz`

创建用户`useradd -M -s /sbin/nologin rabbitmq`

将目录放在`/usr/local/`下, 并重命名
`mv rabbitmq_server-3.7.7 /usr/local/rabbitmq`

将执行文件链接到`/usr/bin`下:
```
ln -s /usr/local/rabbitmq/sbin/cuttlefish /usr/bin/
ln -s /usr/local/rabbitmq/sbin/rabbitmqctl /usr/bin/
ln -s /usr/local/rabbitmq/sbin/rabbitmq-defaults /usr/bin/
ln -s /usr/local/rabbitmq/sbin/rabbitmq-diagnostics /usr/bin/
ln -s /usr/local/rabbitmq/sbin/rabbitmq-env /usr/bin/
ln -s /usr/local/rabbitmq/sbin/rabbitmq-plugins /usr/bin/
ln -s /usr/local/rabbitmq/sbin/rabbitmq-server /usr/bin/
```

在`/usr/local/rabbitmq/etc/rabbitmq/`目录下创建文件

`vim /usr/local/rabbitmq/etc/rabbitmq/rabbitmq-env.conf`, 添加以下内容:
```
RABBITMQ_MNESIA_BASE=/data/rabbitmq
RABBITMQ_LOG_BASE=/var/lib/log
RABBITMQ_LOGS=/var/lib/log/rabbitmq.log
RABBITMQ_SASL_LOGS=/var/lib/log/rabbitmq-sasl.log
```

`vim /usr/local/rabbitmq/etc/rabbitmq/rabbitmq.config`, 添加以下内容:
```
[
    { rabbit, [
        { loopback_users, [ ] },
        { tcp_listeners, [ 5672 ] },
        { ssl_listeners, [ ] },
        { hipe_compile, false },
        { log, [{categories, [{upgrade, [{level, none}] }] }] }
    ] },
    { rabbitmq_management, [ { listener, [
        { port, 15672 },
        { ssl, false }
    ] } ] }
].
```

`vim /usr/local/rabbitmq/etc/rabbitmq/enabled_plugins`, 添加以下内容:
```
[rabbitmq_management].
```

修改这几个文件的权限`chown rabbitmq.rabbitmq /usr/local/rabbitmq/etc/rabbitmq/rabbitmq.config /usr/local/rabbitmq/etc/rabbitmq/rabbitmq-env.conf /usr/local/rabbitmq/etc/rabbitmq/enabled_plugins`

创建`data`目录和`log`目录
```
mkdir -p /data/rabbitmq
mkdir -p /var/lib/log
```

## 启动Rabbitmq

`rabbitmq-server`, 后台启动`rabbitmq-server -detached`
