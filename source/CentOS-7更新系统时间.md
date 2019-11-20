---
title: CentOS 7更新系统时间
copyright: true
date: 2018-05-06 09:42:35
tags: 
  - CentOS
  - ntp
  - ntpdate
  - timedatectl
categories:
---

# `CentOS 7`更新系统时间

`Linux`系统在安装的时候，总是会出现时区，时间的错误。

将`Linux`系统时间和本地区网络时间同步，`ntpdate`可以从网络同步时间，

需要安装`sudo yum install ntp ntpdate`，设置开机自启`sudo systemctl start ntpd`，`sudo systemctl enable ntpd`。

`ntp`只能同步时间，使用`timedatectl`设置时区，`timedatectl`命令在`systemd`包中，默认安装。

`timedatectl`可以查看时间和时区，`timedatectl list-timezones`查看所有的时区，`timedatectl set-zone Asia/Shanghai`设置时区为上海，执行`ntpdate`同步`Asia/Shanghai`的时间。
