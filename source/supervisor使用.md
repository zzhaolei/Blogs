---
title: supervisor使用
copyright: true
date: 2018-08-14 14:19:00
tags:
  - supervisor
  - CentOS
  - Monitor
categories:
---

# supervisor

`supervisor`管理进程，是通过`fork/exec`的方式将被管理的进程当作`supervisor`的子进程来启动。被管理的进程视为`supervisor`的子进程，若是子进程异常中断，则父进程可以获取到子进程的异常中断的信息，通过在配置文件中设置`autostart=true`，可以自动重启异常中断的子进程。

## 安装supervisor

检测是否已经安装`rpm -qa | grep supervisor`, 如果有输出则已经安装，如果没有安装则安装

`supervisor`安装需要安装`epel-release`

 - CentOS/Fedora:
    `yum install epel-release -y`
    `yum install supervisor -y`

## 配置

默认的配置文件是`/etc/supervisord.conf`，默认的配置文件会加载`/etc/supervisord.d/`文件夹下的`*.ini`文件，

`ini`文件的示例：
```
# 程序名
[program:example]

# 执行的命令
command=nohup python test.py >> /dev/null 2>&1 &

# 执行命令的目录
directory=/home/test/example/

# 自动开启
autostart=true

# 自动重启
autorestart=true

# 用户
user=root

# 进程数
numprocs=3

# 启动三秒没有问题，就是成功了
startsecs=3

# 把stderr重定向到stdout，默认false
redirect_stderr=true

# stdout日志文件大小，默认50MB
stdout_logfile_maxbytes=20MB

# stdout日志文件备份数
stdout_logfile_backups=20

# 错误日志
stderr_logfile=/var/log/example_err.log

# 输出日志
stdout_logfile=/var/log/example_out.log

# 默认为false,进程被杀死时，是否向这个进程组发送stop信号，包括子进程
stopasgroup=false

# 默认为false，向进程组发送kill信号，包括子进程
killasgroup=false
```

## 启动

 - 指定配置文件启动
    `supervisorctl -c /etc/supervisord.conf`

 - 使用systemd启动
    `systemctl start supervisord`

 - 重新加载添加的配置文件
    `supervisorctl reload`

### 控制进程

 - 控制所有
    `supervisorctl [ start | restart | stop ] all`

 - 控制指定
    `supervisorctl [ start | restart | stop ] example`

