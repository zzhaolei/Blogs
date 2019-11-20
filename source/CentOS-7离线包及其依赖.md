---
title: CentOS 7离线包及其依赖
copyright: true
date: 2018-05-06 09:43:17
tags:
  - CentOS
  - yum
categories:
---

# CentOS 7离线包及其依赖

推荐使用`yumdownloader --resolve --destdir=path python-pip`，`--resolve`下载所有依赖，`--destdir`指定软件包存放路径，`yumdownloader`在`yum-utils`包中。

而`yum install --downloadonly --downloaddir=path`, 也可以下载依赖, 但是如果软件包已经安装, 就不会再下载，需要安装`yum-plugin-downloadonly`，

## 安装`epel-release`仓库, 这个仓库中有很多默认仓库没有的软件
`yum install -y epel-release`

## 搭建本地仓库
安装`sudo yum install -y createrepo`。

在包所在目录下执行`createrepo ./`。

再编辑文件`sudo vim /etc/yum.repos.d/local.repo`，
```
[Local]
name=Local Yum
baseurl=file:///root/package/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
enabled=1
```

`baseurl`是存放本地包的路径。执行`sudo yum clean all`和`sudo yum makecache`重建缓存。
