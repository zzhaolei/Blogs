---
title: Mac安装MySQL-python报错解决
copyright: true
date: 2018-08-15 17:07:48
tags:
  - Mac OS X
  - MySQL-python
categories:
---

# Mac安装MySQL-pyhton报错

今天在`Mac`上安装`MySQL-python`报错，搜遍网络都说什么`mysql config`路径问题，但是都不行。

## 解决方案

一开始遇到的问题是：
```
Complete output from command python setup.py egg_info:
sh: mysql_config: command not found
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/private/var/folders/jy/7ybw8dpj71n9yhk_xj5jttxc0000gn/T/pip-install-NknW3b/MySQL-python/setup.py", line 17, in <module>
    metadata, options = get_config()
  File "setup_posix.py", line 43, in get_config
    libs = mysql_config("libs_r")
  File "setup_posix.py", line 25, in mysql_config
    raise EnvironmentError("%s not found" % (mysql_config.path,))
EnvironmentError: mysql_config not found

----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /private/var/folders/jy/7ybw8dpj71n9yhk_xj5jttxc0000gn/T/pip-install-NknW3b/MySQL-python/
```

找了一下发现是应为没有安装`mysql-connector-c`，安装`brew install mysql-connector-c`，
再次安装`MySQL-python`，发现还是报错，但是报错信息变了：
```
Complete output from command python setup.py egg_info:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/private/var/folders/jy/7ybw8dpj71n9yhk_xj5jttxc0000gn/T/pip-install-u39xuY/MySQL-python/setup.py", line 17, in <module>
    metadata, options = get_config()
  File "setup_posix.py", line 53, in get_config
    libraries = [ dequote(i[2:]) for i in libs if i.startswith(compiler_flag("l")) ]
  File "setup_posix.py", line 8, in dequote
    if s[0] in "\"'" and s[0] == s[-1]:
IndexError: string index out of range

----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /private/var/folders/jy/7ybw8dpj71n9yhk_xj5jttxc0000gn/T/pip-install-u39xuY/MySQL-python/
```

网上搜了一圈，但是解决方案都不对，最后发现是`mysql-connector-c`本身的配置问题，修改文件

`/usr/local/Cellar/mysql-connector-c/6.1.11/bin/mysql_config`:
```
cd /usr/local/Cellar/mysql-connector-c/6.1.11/bin/
# 修改之前先备份，防止改坏了
cp mysql_config mysql_config.bak
# 修改文件
sudo vim mysql_config

# 将对应的内容修改为
# Create options
libs="-L$pkglibdir"
# libs="$libs -l "
libs="$libs -lmysqlclient -lssl -lcrypto "

```

再次安装`pip install MySQL-python`, 成功
