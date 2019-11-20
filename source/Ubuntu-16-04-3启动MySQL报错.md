---
title: Ubuntu 16.04.3启动MySQL报错
copyright: true
date: 2018-05-06 09:38:11
tags:
  - Ubuntu
  - MySQL
categories:
---

# Ubuntu 16.04.3启动MySQL报错

今天安装`mysql`，连接`MySQL`时报错`mysql: [Warning] Using a password on the command line interface can be insecure. ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)`

通过`ls`命令查看，发现`/var/run/mysqld`目录并不存在,创建这个文件夹`sudo mkdir /var/run/mysqld`,然后修改文件夹的所属者`sudo chown mysql:mysql /var/run/mysqld`.

重启`MySQL`服务`sudo /etc/init.d/mysql restart`

再次尝试连接`MySQL mysql -uroot -p`, 成功。

