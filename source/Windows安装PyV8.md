---
title: Windows安装PyV8
copyright: true
date: 2018-05-06 10:02:39
tags:
  - Windows
  - PyV8
categories:
---

# Windows上安装PyV8

在`PyPi`网站上有`Windows`的`exe`格式的包连接, [PyPi](https://pypi.org/project/PyV8/),

`Google`注意网络是否通畅!

官网地址 [Google PyV8](https://code.google.com/archive/p/pyv8/downloads)

双击安装, 注意, 一般会自动检测Python安装目录(一般是`C:\Python27`, 如果你没有改安装目录, 检测的是path路径), 一直下一步安装即可.

如果使用的是虚拟环境的话, 可以将`C:\Python27\Lib\site-packages\`下的`PyV8-xxx-py2.7.egg-info、PyV8.py、PyV8.pyc、PyV8.pyo`复制到虚拟目录的`Lib\site-packages\`下.

# 使用

```
from PyV8 import JSContext


# 创建一个对象并进入
js = JSContext()
js.__enter__()

# jsFile为要执行的代码, 是字符串
js.eval(jsFile)

# 调用js中的方法
# 将func绑定到js中的Func
func = js.locals.Func

# 执行
print func()

# 调用js中的变量
# result为js中的变量名
result = str(js.eval('result'))

```

