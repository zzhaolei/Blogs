---
title: Mac上使用ssh连接服务器title显示服务器的ip
copyright: true
date: 2018-12-18 18:11:26
tags:
categories:
---

# Mac上使用ssh连接服务器title显示服务器的ip
使用`Mac`开发时，管理的服务器过多时，会搞混乱。可能有时啪啪啪一顿操作，最后发现操作错了机器。

 - 解决方案
在远程服务器上，编辑`vim /etc/profile` `(为所有用户均做此设定。 为单个用户设定，编辑vim ~/.bashrc)`, 在文件最后追加:
```
# 当通过ssh连接，并且不是Linux tty登录的时候，设置环境变量，写入/etc/profile中，为所有用户设置
if [ "$SSH_CONNECTION" != '' -a "$TERM" != "linux" ];then
    declare -a HOSTIP
    HOSTIP=`echo $SSH_CONNECTION |awk '{print $3}'`
    export PROMPT_COMMAND='echo -ne "\033]0;${USER}@$HOSTIP:[${HOSTNAME%%.*}]:${PWD/#$HOME/~} \007"'
fi
```

保存并退出。

如果编辑`~/.bashrc`, 需要执行一下`source ~/.bashrc`
