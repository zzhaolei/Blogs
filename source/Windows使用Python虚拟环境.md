---
title: Windows使用Python虚拟环境
copyright: true
date: 2018-05-06 10:03:43
tags:
  - Python
  - Windows
categories:
---

# Windows使用virtualenv和virtualenvwrapper-win

在`Windows`上使用`virtualenv`进行多版本`Python`隔离.

## 安装`Python`

在`Python`官网下载`Python 3.X or Python 2.X`, 双击安装`记得勾选Add to Path`,

在`cmd`中输入`Python`, 测试`Path`路径是否正确, 如果没有进入`Python shell`中, 进入`Python`安装目录, 复制路径, 右键`此电脑-->属性-->高级系统设置-->环境变量-->系统变量-->Path-->编辑-->新建-->粘贴`最后加一个`\`.

在`cmd`中再次测试.

## 安装virtualenv和virtualenvwrapper-win

`pip install virtualenv`

`Windows`使用`virtualenvwrapper-win`, 
`pip install virtualenvwrapper-win`

添加`WORKON_HOME`路径, 右键`此电脑-->属性-->高级系统设置-->环境变量-->系统变量-->新建`, 变量名为`WORKON_HOME`, 变量值为`创建的虚拟目录的路径`.

## 创建虚拟目录

在`WORKON_HOME`目录中, 使用`virtualenv --no-site-package py2`, 如果不使用`virtualenvwrapper-win`, 需要进入`py2/Scripts/`下面, 执行`activate.bat`, 退出需要执行`deactivate.bat`.

使用`workon py2`可以直接进入虚拟环境.

