---
title: rbash限制用户执行的命令
copyright: true
date: 2018-05-06 09:59:06
tags:
  - bash
  - rbash
categories:
---

# rbash限制用户执行的命令

# 软连接
```
sudo ln -s /bin/bash /bin/rbash
sudo bash -c 'echo "/bin/rbash" >> /etc/shells'
sudo chsh -s /bin/rbash $ruser
```

建一个直接使用`/bin/rbash`的用户
`useradd -s /bin/rbash $ruser`

ruser是需要限制的用户名

在这个用户目录下创建一个`.bin`，存放用户可以执行的命令
`mkdir -p /home/$ruser/.bin`
通过软连接执行命令
`ln -s /bin/clear /home/$ruser/.bin/clear`

编辑文件`/home/$ruser/.bashrc`,添加`export PATH=$HOME/.bin/`.
文件一定要以`# .bashrc`为首行.

示例
```
#!/bin/bash

# rbash脚本
create_user()
{
    if [ ! -f "/bin/rbash" ];then
        ln -s /bin/bash /bin/rbash
    fi

    count=`cat /etc/shells | grep /bin/rbash | wc -l`
    if [ $count -eq 0 ];then
        bash -c 'echo "/bin/rbash" >> /etc/shells'
    fi

    count=`cat /etc/passwd | grep $user | wc -l`
    if [ $count != 0 ];then
        # -n参数, 不换行输出
        echo -n "[ The user already exists, modify the shell? ] "
        read flag
        if [ $flag == "y" ];then
            chsh -s /bin/rbash $user
        fi
    else
        aa=`openssl passwd -1 $password`
        useradd -s /bin/rbash -p $aa -d /home/$user $user
    fi

    if [ ! -d "/home/$user/bin" ];then
        mkdir -p /home/$user/bin
    fi

}

# 添加相应的命令的软连接
add_command()
{
    for i in $com;do
        count=`ls /home/$user/bin/ | grep "$i" | wc -l`
        if [ $count -eq 0 ];then
            path=`which $i`
            ln -s $path /home/$user/bin/$i
        fi
    done
}

bash_profile()
{
    echo "export PATH=/home/$user/bin/" >> /home/$user/.bashrc
}

info()
{
    echo "Restricted Rbash!" > /etc/motd
}

clear_history()
{
    # 清空历史记录
    echo "" > ~/.bash_history
    history -c
}

p_v()
{
    if [ $UID != 0 ];then
        echo "You need to be root to perform this command."
        exit 1
    fi
    if [ "$user" == "" ];then
        echo "Please enter user name."
        exit 1
    fi
    if [ "$password" == "" ];then
        echo "Please enter user password."
        exit 1
    fi 
}

user=$1
password=$2

main()
{
    # 允许的命令, 用空格进行分隔
    # cd命令在rbash上是受限的, 建立软连接也不能执行
    com="ifconfig ip ls"

    p_v
    
    # 如果用户已存在注释create_user和bash_profile
    create_user
    add_command
    bash_profile
    # info
    clear_history
}

main

```

