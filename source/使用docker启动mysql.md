---
title: 使用docker启动mysql
copyright: true
date: 2018-05-06 10:06:46
tags:
  - docker
  - MySQL
categories:
---

# 使用docker启动mysql

安装`docker`

查询`mysql`版本, 
`docker search mysql`

拉取指定版本的镜像

`docker pull NAME`

启动镜像, 第一次启动最少需要指定`MYSQL_ROOT_PASSWORD`

`docker run -d -e MYSQL_ROOT_PASSWORD=password -p 3306:3306 --privileged=true --name mysql mysql_name`

`--privileged=true`是严格模式启动容器, 需要修改容器文件就要启用

如果是整个替换文件最好查看好文件的权限, mysql的配置文件第一个读取的是`/etc/my.cnf`, 权限是`777 root:root`.

## 修改配置文件

`docker run -d -e MYSQL_ROOT_PASSWORD='password' -v $HOME/my:/etc/mysql/conf.d/ --name mysql mysql57:latest`

`$HOME/my`这个文件夹是存放`my.cnf`配置文件

