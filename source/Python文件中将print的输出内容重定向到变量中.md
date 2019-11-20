---
title: Python文件中将print的输出内容重定向到变量中
copyright: true
date: 2018-05-06 09:58:38
tags:
  - Python
  - Redirection
  - sys
categories:
---

# Python文件中将print的输出内容重定向到变量中

有时候需要用到别人的代码, 但是又不想修改别人的文件, 想拿到输出的结果, 这时候就需要使用`sys`模块, 将`print`输出的内容重定向到变量中.

`Python`调用`sys`模块中的`sys.stdout`, 实际上是调用的`sys.stdout.write`方法, 这样如果操作的对象是文件就不会有问题.
```
import sys
# 保存当前的sys.stdout状态, 开始捕获当前的输出
current = sys.stdout
f = open(path, 'w')
# 这一步实际是sys.stdout.write, 当sys捕获到了print输出的时候, 就写入f里面
sys.stdout = f
print 'test'
# 恢复状态, 之后的print内容都不捕获了
sys.stdout = current
```

但是当把`f`文件对象改为一个变量时, 就会报错`AttributeError: 'str' object has no attribute 'write'`, 原因是变量没有`write`方法, 所以根据这个, 我们可以重写一个方法.

```
import sys

class __Autonomy__(object):
    """
    自定义变量的write方法
    """
    def __init__(self):
        """
        init
        """
        self._buff = ""

    def write(self, out_stream):
        """
        :param out_stream:
        :return:
        """
        self._buff += out_stream


current = sys.stdout
a = __Autonomy__()
# 会调用a的write方法, 和self._buff的内容拼接
sys.stdout = a
print 'test'
sys.stdout = current

# 输出捕获的内容
print a._buff
```

