---
title: Python将mongodb导出的bson文件转为字典对象
copyright: true
date: 2018-05-06 09:56:28
tags:
  - Python
  - mongodb
  - bson
categories:
---

# Python将mongodb导出的bson文件转为字典对象

安装`bson`包, `sudo pip install bson`

示例
```
# 解决编码问题
import sys
reload(sys)
sys.setdefaultencoding('utf8')

# 导入bson模块
import bson

# 通过rb的方式读取文件
bson_file = open('./xunfeng/db/Config.bson', 'rb')

# loads接收的是字符串
data = bson.loads(bson_file.read())
```

如果不进行编码会报错`UnicodeDecodeError: 'ascii' codec can't decode byte 0xe5 in position 0: ordinal not in range(128)`

主要是因为`bson`文件中可能包含中文.

`Python2`的编码问题真恶心.

