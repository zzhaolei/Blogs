---
title: 使用jinja2模板引擎生成html文件
copyright: true
date: 2018-05-06 10:08:29
tags:
  - jinja2
  - html
categories:
---

# 使用jinja2模板引擎生成html文件

`jinja2`是纯`Python`的模板引擎，是仿照`Django`模板的`Python`模板语言。
它速度快，被广泛使用，提供了可选的沙箱模板执行环境保证安全。

使用`pip`安装`sudo pip install jinja2`, `Windows`上使用管理员运行命令提示符，`pip install jinja2`,不需要使用`sudo`提权。

## 直接加载`html`语言

```
from jinja2 import Template

template = Template("hello {{ name }}")

template.render(name = 'world')
```
render可以指定对指定的参数进行赋值。

## 使用本地文件

```
import jinja2

# 配置jinja2在本地文件系统的搜索路径
TemplateLoader = jinja2.FileSystemLoader(searchpath='$\PATH')

# 环境变量
env = TemplateLoader(loader=TemplateLoader)

# 加载模板
template = env.get_template('PATH')
template.render(name = "world")
```

