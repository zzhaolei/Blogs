---
title: Windows使用Cmder
copyright: true
date: 2018-05-06 10:03:04
tags:
  - Windows
  - Cmder
categories:
---

# Windows使用Cmder

`cmder`是一个增强型命令行工具, 不仅可以使用`Windows`下的所有命令, 更爽的是可以使用`linux`的命令, `shell`命令。

## 下载
官网地址: [cmder官网](http://cmder.net/)

下载的时候, 会有两个版本, 分别是`mini`与`full`版: 唯一的差别在于有没有内建 `msysgit`工具, 这是`Git for Windows`的标准配备: 全安装版`cmder`自带了`msysgit`, 除了`git`本身这个命令之外, 里面可以使用大量的`linux`命令: 比如`grep`, `curl`(没有`wget`), 像`vim`, `grep`, `tar`, `unzip`, `ssh`, `ls`, `bash`, `perl`对于爱折腾的`Coder`来说更是痛点需求。

直接解压到某个目录就可以了, 点击`Cmder.exe`运行。

## 配置环境变量

`新建系统变量`

  - 变量名: `CMDER_HOME`
  - 变量值: `安装绝对路径`

保存。

编辑Path, 添加`%CMDER_HOME%`.

## 配置cmder右键菜单

配置环境变量后, 在管理员权限的终端输入以下语句。
`Windows 8`或者`Windows 10`可以直接`win + x`再按`a`键进入。
输入`Cmder.exe /REGISTER ALL`.

## 新标签打开个管理员权限终端

启动终端后, 使用快捷键`Ctrl + t`, 新建`console`, 在`Create new console`中输入`{bash::bash as Admin}`, 勾选右下角的`Run as administator`, 点击`Start`按钮

## 设置

使用快捷键打开设置`win + alt + p`.

设置`bash`作为默认开启的选项, 点击左侧的`Startup`, 在`Specified named task`的下拉列表中选择`{bash::bash as Admin}`, 点击`Save settings`.

## 解决中文乱码

在设置中
`Settings->Startup->Environment` 添加
`set LANG=zh_CN.UTF-8`
`set LC_ALL=zh_CN.utf8`

## 使用WSL的vim方向键失效的问题

在`Settings-->Startup-->Command line`的值改为`%windir%\system32\wsl.exe ~ -cur_console:p:n`

## 将命令提示符λ改成$

进入`Cmder.exe`所在的目录, 进入`vendor`, 打开`clink.lua`文件, 
搜索`λ`符号, 将搜索到的`λ`修改为`$`

保存重启

