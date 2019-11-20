---
title: Xshell连接WSL
copyright: true
date: 2018-05-06 10:04:56
tags:
  - Xshell
  - WSL
categories:
---

# Xshell连接WSL

`Windows`的`Windows Subsystem for Linux`很好用, 可以直接使用`Linux`的`CLI`模式, 对于开发来说很友好.

## 安装

`Windows 10`系统上, 使用快捷键`win + x`, 点击`应用和功能`, `最大化显示`(`Windows 10`的设置是自适应的, 如果不最大化, 可能会无法显示全部功能), 点击`右侧 程序和功能`, 在弹出的窗口左侧选择`启用或关闭Windows功能`, 在弹出的窗口中, 找到`适用于Linux的Windows子系统`, 重启系统.

`Windows store`中搜索`Linux`, 会有`Ubuntu、Kali、Debian、openSUSE`等发行版, 随便点击一个发行版, 例如`Ubuntu`, 在新页面点击`获取`.

## 启动

点击左下角的`windows`按钮, 在最近添加中点击刚安装的系统图标, 会有个初始化的过程, 之后输入用户名和密码, 就完成了.

通过`sudo apt update && sudo apt upgrade -y`更新系统, 安装`ssh`服务`sudo apt autoremove --purge openssh-server -y && sudo apt install openssh-server -y`

编辑文件`vim /etc/ssh/sshd_config`, `Windows`和子系统共用端口, 修改端口, 
```
# 端口
Port 6666
UsePrivilegeSeparation no
# 密码验证
PasswordAuthentication yes
# 允许登陆的用户
AllowUsers name
```

重启服务`sudo service ssh --full-restart`

生成`key`, `ssh-keygen -t rsa`一路回车.

## 连接

在`xshell`中使用`Alt + N`创建新的连接会话, `主机`改为`127.0.0.1`, `端口`改为自己设置的端口.

点击左侧的`用户身份验证`, 方法使用`Password`, 用户名和密码为`Ubuntu`用户名密码, 点击连接.

## 开机自启

`Bash`窗口不能关闭, 如果关闭了, 那么相当于系统关机.


可以使用`windows`自带的定时任务计划上添加开机启动`Ubuntu ssh`服务的任务。
搜索`任务计划程序`, `操作 --> 创建基本任务`
配置：
```
触发器: 当计算机启动时
操作: 启动程序
程序或脚本: C:\Windows\System32\bash.exe
参数为: -c \"sudo /usr/sbin/sshd -D\"

选择`当单击“完成”是, 打开此任务属性的对话框`按钮，点击完成，打开属性页
在属性页选择"使用最高权限运行"，避免错误。
```

