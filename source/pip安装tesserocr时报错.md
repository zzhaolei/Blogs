---
title: pip安装tesserocr时报错
copyright: true
date: 2018-05-06 09:38:47
tags:
  - pip
categories:
---

# pip安装tesserocr时报错

在`Xubuntu`上的`python2`虚拟环境中，
使用`pip`安装`tesserocr`时报错`error: command 'x86_64-linux-gnu-gcc' failed with exit status 1`,

一般是因为缺少`libtesseract-dev`, 在终端输入以下命令安装即可：`sudo apt install libtesseract-dev`

如果安装之后没有解决问题，可以输入以下命令安装类库：

`sudo apt-get install libxml2-dev libxslt1-dev python-dev zlib1g-dev libevent-dev`

如果有的类库已经安装过了，`apt`会自动跳过安装

