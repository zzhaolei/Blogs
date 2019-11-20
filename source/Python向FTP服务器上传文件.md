---
title: Python向FTP服务器上传文件
copyright: true
date: 2018-07-25 21:09:55
tags:
categories:
---

# Python向FTP服务器上传文件

## 上传

代码示例:
```
#!/usr/bin/python
# -*- coding:utf-8 -*-

from ftplib import FTP


ftp = FTP()

# 打开调试级别2, 显示详细信息
ftp.set_debuglevel(2)

# 服务器IP和端口
ftp.connect("192.168.10.2", "21")

# 匿名登陆, 如果需要登陆, 就把两个空字符串写上用户名和密码就行了("username", "password")
ftp.login("", "")

# 切换目录, 相对于ftp目录, 比如设置的ftp根目录为/vat/ftp, 那么pub就是/var/ftp下面的目录
ftp.cwd("pub")

# 查看目录下有哪些文件, 如果文件名已经存在, 那么再次上传同一个文件就会报错, 返回列表
ftp.nlst("/pub")

# 使用二进制的方式打开文件
f = open("/root/test.py", 'rb')

# 上传文件, bufsize缓冲区大小
ftp.storbinary("STOR {}".format("test.py"), f [, bufsize])
f.close()

# 关闭调试模式
ftp.set_debuglevel(0)

# 退出FTP连接
ftp.quit()
```

## 下载

代码示例:
```
#!/usr/bin/python
# -*- coding:utf-8 -*-

from ftplib import FTP


ftp = FTP()

# 打开调试级别2, 显示详细信息
ftp.set_debuglevel(2)

# 服务器IP和端口
ftp.connect("192.168.10.2", "21")

# 匿名登陆, 如果需要登陆, 就把两个空字符串写上用户名和密码就行了("username", "password")
ftp.login("", "")

# 切换目录, 相对于ftp目录, 比如设置的ftp根目录为/vat/ftp, 那么pub就是/var/ftp下面的目录
ftp.cwd("pub")

# 查看目录下有哪些文件, 如果文件名已经存在, 那么再次上传同一个文件就会报错, 返回列表
ftp.nlst("/pub")

# 以二进制写模式在本地打开文件
f = open("test.py", "wb").write

# 下载文件, bufsize缓冲区大小
ftp.retrbinaly("RETR test.py", f [, bufsize])
f.close()

# 关闭调试模式
ftp.set_debuglevel(0)

# 退出FTP连接
ftp.quit()
```
