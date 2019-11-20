---
title: python argparse模块的使用
copyright: true
date: 2018-05-06 09:54:39
tags:
  - Python
  - argparse
categories:
---

# python argparse模块的使用

```
import argparse

def get_parse():
    # 初始化
    parse = argparse.ArgumentParser()
    # 添加选项，类型为str，默认为空
    parse.add_argument('-u', type=str, default='')

    args = parse.parse_args()
    # args = parse.parse_know_args()

    # vars将对象转为字典
    return vars(args)

# 使用
args = get_parse()
url = args['u']

```

