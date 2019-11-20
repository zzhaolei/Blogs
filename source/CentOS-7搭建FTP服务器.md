---
title: CentOS 7搭建FTP服务器
copyright: true
date: 2018-07-25 20:49:19
tags:
  - CentOS 7
  - vsftpd
  - selinux
categories:
---

# CentOS 7搭建FTP服务器

## 安装vsftpd

 - CentOS/Fedora
    `yum install vsftpd -y`

## 编辑vsftpd配置文件

默认`vsftpd`的配置文件在`/etc/vsftpd/`目录下

默认`ftp`是允许匿名登陆的, `ftp`目录在`/var/ftp`.

## 启动ftp服务

`systemctl start vsftpd`

## 配置ftp上传和下载

编辑一个系统文件之前, 记得备份, `cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak`

编辑配置文件`vim /etc/vsftpd/vsftpd.conf`, 修改对应的字段, 如果没有相应的字段, 就新建:
```
# 允许匿名用户和本地用户登陆
anonymous_enable=YES
local_enable=YES

# 使用匿名登入时，所登入的目录
anon_root=/home/ftp

# 如果设为YES, 允许匿名登入者有上传文件（非目录）的权限. 默认值为NO.
anon_upload_enable=YES

# 如果设为YES, 允许匿名登入者有新增目录的权限, 只有在write_enable=YES时, 此项才有效. 当然, 匿名用户必须要有对上层目录的写入权. 默认值为NO.
anon_mkdir_write_enable=NO

# 如果设为YES, 允许匿名登入者更多于上传或者建立目录之外的权限. 默认值为NO.
anon_other_write_enable=NO

# 设置是否改变匿名用户上传文件(非目录)的属主. 默认值为NO.
chown_uploads=NO

# 设置匿名登入者新增或上传档案时的umask值. 默认值为077, 则新建档案的对应权限为700
anon_umask=077
```

## 如果不能上传和下载

 - 配置文件

```
# 允许匿名用户登录FTP
anonymous_enable=YES

# 设置匿名用户的登录目录
anon_root=/var/ftp/pub

# 打开匿名用户的上传权限
anon_upload_enable=YES

#开启匿名用户的下载权限
anon_world_readable_only=no 

# 打开匿名用户创建目录的权限
anon_mkdir_write_enable=YES

# 打开匿名用户删除和重命名的权限
anon_other_write_enable=YES

#匿名用户的掩码(含义: 如umask是022, 这时创建一个权限为666的文件, 文件的实际权限为666-022=644)
anon_umask=022
```

 - 目录权限设置

    默认情况下, `ftp`的根目录为`/var/ftp`, 为了安全, 这个目录默认不允许设置为`777`权限, 否则`ftp`将无法访问. 但是我们要匿名上传文件, 需要`other`用户的写权限, 正确的做法"

    在`/var/ftp`中默认有一个`pub`文件夹, 将个文件夹权限设置为`777`(`视具体需要自己设`), 在`pub`这个文件夹中, 匿名用户可以上传文件, 创建文件夹, 删除文件等.

 - SELinux配置

如果还是不行, 那么查看`selinux`的配置, 查看配置文件`/etc/selinux/config`中的`SELINUX=disabled`是不是`disabled`, 如果是, 那么使用命令`getenforce`查看状态, 如果是`Permissive`(这是`SELinux`的中间状态), 那么重启系统(命令`reboot`)就好了.

再次使用`getenforce`查看状态, 就会变成`Disabled`
