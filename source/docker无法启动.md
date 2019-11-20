---
title: docker无法启动
copyright: true
date: 2018-05-06 09:45:07
tags:
  - docker
categories:
---

# docker无法启动

`docker`启动时报错`Error starting daemon: SELinux is not supported with the overlay2 graph driver on this kernel. Either boot into a newer kernel or disable selinux in docker (--selinux-enabled=false)`

修改`sudo vim /etc/sysconfig/docker`, 中的`--selinux-enabled`为`--selinux-enabled=false`

如果需要, ~~可以修改`/etc/selinux/config`中`SELINUX`的值为`disabled`.~~, 不强求

