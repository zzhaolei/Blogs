---
title: CentOS 7安装Python3
copyright: true
date: 2018-05-06 09:41:43
tags:
  - CentOS
  - Python
  - tar
  - ius
  - virtualenv
  - virtualenvwrapper
categories:
---

# 在CentOS 7上安装Python3

## 源码包安装
`Python`官方没有对`Linux`打包, 需要下载源码包进行编译.

可以自己下载最新版本的`Python`, 在[Python](https://www.python.org/downloads/)下载页面, 点击`Python3.x.x`版本对应的`Download`, 在新页面的`Files`下, 选择`Source release`, 选择`Gzipped source tarball`或者`XZ compressed source tarball`压缩方式的包, 我选择的是`tgz`压缩.

[Python3.6.5](`https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz`)

### 下载源码包
`wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz`

### 解压
`tar -xzvf Python-3.6.5.tgz`

### 安装依赖
`yum install zlib zlib-devel libffi-devel -y`

### 编译
```
cd Python-3.6.5
./configure --prefix=/usr/local/src/python3.6 --enable-optimizations
make
make install
```

`--enable-optimizations`是优化选项（`LTO,PGO`等）加上这个`flag`编译后，性能有`10%`左右的优化，但是这会明显的增加编译时间。

会安装在`/usr/local/python3.6`, 在`/bin`下和`/usr/local/bin`下有`python3.6`的可执行文件, 如果做软连接到`python3`, 在执行`python3`的时候会报错.


## 使用源安装

安装`ius`源
```
# 依赖epel
yum install epel-release
yum install https://centos7.iuscommunity.org/ius-release.rpm
```

### 安装Python
IUS软件包的命名方式为`软件名+版本号+次版本号+u`, `Python`的包名为`python36u`.

```
yum install python36u python36u-pip python36u-devel
```

## 安装虚拟环境

```
pip3.6 install virtualenv virtualenvwrapper
```

编辑`home`目录下的`.bashrc`或`.zshrc`(`.zshrc`是`zsh`的配置文件),
`vi ~/.bashrc`, 在文件最后添加(`$HOME/.virtual`是虚拟环境的路径, 自定义):
```
WORKON_HOME=$HOME/.virtual
source /usr/bin/virtualenvwrapper.sh
```

保存退出, 在`shell`中执行`source .bashrc`, 进入虚拟环境目录`cd ~/.virtual`, 执行
```
virtualenv -p /usr/bin/python3.6 --no-site-packages py3
```

`-p`是生成指定`python`版本的虚拟环境, 是个路径

`--no-site-packages`是不将系统`Python`的库包复制到虚拟环境中.

`py3`是虚拟环境的名称, 可以自定义

使用`workon py3`进入虚拟环境.
