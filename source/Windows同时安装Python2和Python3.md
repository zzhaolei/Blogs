---
title: Windows同时安装Python2和Python3
copyright: true
date: 2018-06-09 22:15:34
tags:
  - Windows
  - Python2
  - Python3
categories:
---

# Windows同时安装Python2和Python3

在`Python`官网下载最新的`Python2`和`Python3`的安装包, 安装时记得勾选`Add Python.xx to Path`.

这样两者都会在环境变量中, 但是`Python2`和`Python3`的软件名称一样的, 所以在调用的时候, 只会调用最后安装的.

## 解决方案
分别在两者对应的安装目录下修改软件名称, 比如将`Python3`的`python.exe`和`pythonw.exe`改为`python3.exe`和`pythonw3.exe`.

这个时候`pip`名称还是一致的, 通过卸载重装的方式更新`pip`:
```
python -m pip install --upgrade pip --force-restart
python3 -m pip install --upgrade pip --force-restart
```

这个时候在`cmd`中查看就是对应版本的了:
```
pip -V
pip3 -V
```
