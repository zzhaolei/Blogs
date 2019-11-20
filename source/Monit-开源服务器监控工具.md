---
title: Monit-开源服务器监控工具
copyright: true
date: 2018-07-02 15:29:22
tags:
  - Linux
  - Monit
categories:
---

# Monit-开源服务器监控工具

`Monit`是一个用于管理和监控`Unix`系统的小型开源工具. `Monit`进行自动维护和修理, 并且可以在错误情况下执行有意义的因果作用.

比`zabbix`轻量.

## 全局配置 - Web状态页面

 - `monit-5.25`默认监听`2812`
 - 对`web`状态页面的访问是通过`SSL`加密的
 - 使用`admin/monit`作为用户名/口令登录
 - 只允许通过`localhost`、`myhost.mydomain.ro`和在局域网内部`192.168.0.0/16`访问
 - `Monit`使用`pem`格式的`SSL`证书

### 生成一个自签名证书

```
cd /etc/pki/tls/certs/
# 会自动在/etc/ssl/certs/下面复制一份monit.pem
# 默认权限是0400, 如果不是就手动修改
./make-dummy-cert monit.pem
```

### httpd配置

编辑`vi /etc/monitrc`, 修改相应的内容为:
```
set httpd port 2812 and

    # 只接受来自本地主机的连接(only accept connection from localhost)
    use address 10.10.10.141

    # 允许本地主机连接到服务器和(allow localhost to connect to the server and)
    allow localhost
    # 和指定网段(192.168.0.0/16), 或者所有ip都可以访问
    allow 0.0.0.0/0.0.0.0
    
    # 需要用户'admin'，密码为'monit'(require user 'admin' with password 'monit')
    allow admin:monit
    
    # 启用SSL/TLS并设置服务器证书的路径(enable SSL/TLS and set path to server certificate)
    with ssl {
        pemfile: /etc/ssl/certs/monit.pem
    }
```
或者
```
set httpd port 2812 and

    # 只接受来自本地主机的连接(only accept connection from localhost)
    use address 10.10.10.141

    # 允许本地主机连接到服务器和(allow localhost to connect to the server and)
    allow localhost
    # 和指定网段(192.168.0.0/16), 或者所有ip都可以访问
    allow 192.168.0.0/16
    # 配置域名
    allow myhost.mydomain.ro

    # 需要用户'admin'，密码为'monit'(require user 'admin' with password 'monit')
    allow admin:monit

    # 启用SSL/TLS并设置服务器证书的路径(enable SSL/TLS and set path to server certificate)
    with ssl {
        pemfile: /etc/ssl/certs/monit.pem
    }
```

## 全局通知 - 邮件通知

我们至少需要一个可用的`SMTP`服务器来让`Monit`发送邮件.

 - 邮件服务器的机器名: smtp.exmail.qq.com
 - Monit使用的发件人: monit@monit.ro
 - 邮件的收件人: test@monit.ro
 - 邮件服务器使用的SMTP端口: 587(默认是25, 根据自己的SMTP服务器确定)

编辑`vi /etc/monitrc`, 将相应的内容修改为:
```
set mailserver  smtp.exmail.qq.com port 465
set mail-format {
 from: monit@monit.ro
 subject: $SERVICE $EVENT at $DATE on $HOST
 message: Monit $ACTION $SERVICE $EVENT at $DATE on $HOST : $DESCRIPTION.

       Yours sincerely,
          Monit

  }

set alert test@qq.com
```

## 全局配置 - Monit守护进程

可以设置为:
 - 在120秒后进行第一次检测
 - 每2分钟检测一次服务
 - 使用syslog来记录日志

编辑`vi /etc/monitrc`, 将相应的内容修改为:
```
set daemon 60
   with start delay 60
set logfile syslog facility log_daemon
```

~~必须定义`idfile`, `Monit`守护进程的一个独一无二的ID文件; 以及`eventqueue`, 当monit的邮件因为SMTP或者网络故障发不出去, 邮件会暂存在这里; 以及确保`/var/monit`路径是存在的. 然后使用下边的配置就可以了:~~
```
set idfile /var/monit/id
set eventqueue
     basedir /var/monit
```
默认路径为`$HOME/.monit.id`


## 验证全局配置

语法检测, 检测`/etc/monitrc`和`/etc/monit.d`的配置语法是否正确:
```
$ monit -t

New Monit id: 8b7015f050672ebfd066d9e161cdf3ef
Stored in '/root/.monit.id'
Control file syntax OK
```

如果报错, 请检查配置文件.

启动服务, 并设置开机自启:
```
systemctl start monit
systemctl enable monit
```

## 服务监控

### 端口监控

在`/etc/monit.d/`下新增配置文件`monitor`, 内容如下:
```
# 匹配进程名
check process flask MATCHING gunicorn

# 配置服务启动和重启命令
start program = "/usr/bin/sudo service mongod start"
restart program = "/usr/bin/sudo service mongod restart"

# 如果端口27017无法访问则认为服务失败，发报警邮件并重启服务
if failed port 27017  type tcp then alert
if failed port 27017  type tcp then restart

# 如果在三个周期内重启了3次，则不再监控
# if 3 restarts within 3 cycles then unmonitor
```

### 使用脚本监控

在`/etc/monit.d/`下新增配置文件`monitor`, 内容如下:
```
check program monitor with path "/bin/bash /etc/monit.d/service/service" with timeout 60 seconds

# IF STATUS operator value THEN action
if status == 1 then exec "/bin/bash /etc/monit.d/service/service restart views"

```
