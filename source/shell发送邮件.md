---
title: shell发送邮件
copyright: true
date: 2018-07-19 10:43:37
tags:
categories:
---

# 通过shell发送邮件

## 安装mailx

`CentOS 7`自带有`mailx`软件包, 有`/usr/bin/mail`命令, 配置文件为`/etc/mail.rc`.

如果没有软件包, 可以安装
 - CentOS/Fedora
    `yum install -y mailx`

## 修改配置文件

一般的邮件服务都会需要密码, 以及邮件服务器等信息, 修改配置文件, 添加相关信息, 直接修改配置文件`vim /etc/mail.rc`, 在最后追加:
```
# smtp服务器认证的用户名
set from=test@qq.com

# smtp服务器的地址
set smtp=smtp.exmail.qq.com

# 邮件认证的方式
set smtp-auth=login

# smtp服务器认证的用户名
set smtp-auth-user=test@qq.com

# smtp服务器认证的用户密码(授权码)
set smtp-auth-password=test_password

# 忽略验证
set ssl-verify=ignore
set nss-config-dir=/etc/maildbs/
```

## 发送邮件

### 直接在shell中使用mail命令

`mail -s "邮件主题" 收件人地址`

上面的命令回车执行后, 会等待用户的输入, 当输入完成后, 使用`ctrl+D`键, 结束并发送邮件.

### 使用echo和管道的方式

`echo "测试邮件" | mail -s "邮件主题" 收件人地址`

`echo "邮件内容" | mail -s "邮件主题" -a 附件 收件地址`