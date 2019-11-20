---
title: docker相关使用
copyright: true
date: 2018-05-06 09:45:53
tags:
  - docker
categories:
---

# docker相关使用

## 安装docker

在`CentOS 7`上安装`docker-ce`，首先检查系统中是否已经安装过`docker`及相关依赖：

`$ sudo yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine`

通过仓库进行安装，安装所需的包:

`$ sudo yum install yum-utils device-mapper-persistent-data lvm2`

`sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

启用稳定版的仓库`sudo yum-config-manager --enable docker-ce-stable`,
禁用仓库`sudo yum-config-manager --disable docker-ce-stable`

安装`docker-ce`,`sudo yum install docker-ce`.如果出现key相关错误，可以重新安装一下，会使用下载的缓存，并提示导入指纹。

启动`docker`,`sudo systemctl start docker`

现在使用docker相关命令的话，需要`sudo`权限，可以将当前用户添加进入docker组，
`sudo usermod -aG docker $USER`
退出shell，重新进入就不需要`sudo`权限了

修改docker镜像为中国官方镜像`sudo vim /etc/docker/daemon.json`,添加以下内容
`{"registry-mirrors":["https://registry.docker-cn.com"]}`

## 修改镜像名字

`sudo docker tag IMAGEID [docker.my.com:port/]NAME[:1.0.0]`

如果需要上传到指定私有镜像, 需要指定`域名`或`ip`, 版本号根据自己的需求进行添加.

## 上传镜像到私有仓库

需要将镜像改名, `sudo docker tag IMAGEID docker.my.com:port/NAME[:1.0.0]`, 

编辑`sudo vim /etc/docker/daemon.json`, 添加内容`{"insecure-registries":["docker.my.com:port"]}`, 如果之前已经有内容, 可以在已有内容后添加`,`, 然后添加`"insecure-registries":["docker.my.com:port"]`, 重启`sudo systemctl restart docker`

上传`sudo docker push docker.my.com:port/NAME:1.0.0`, 需要指定版本号, 如果重命名时没有指定, 就使用默认的版本号.

~~
## 开机自启容器

在启动容器时需要使用`--restart=always`, `注意`一定要把这个参数放在`-d`参数的`后面`, 不然容器会一直`重启`.
示例`docker run -d --restart=always -p port:port -v /local_path:/docker_path --name name image_name`~~

如果启动时没有指定`--restart=always`, 也可以使用更新命令`docker update --restart=always name`

## 复制docker中的内容

可以使用`sudo docker ps -a`查看是否有`docker`容器以及`NAMES`, 如果容器停止并删除了, 就不能复制内容.

`sudo docker cp NAMES:/home/users/file ./`

## 进入docker的shell

`sudo docker exec -it NAMES /bin/bash`, 和`Linux`的操作一样, 
可以通过`cat /etc/issue`查看`Linux`发行版本, 默认进入是`root`权限.
使用`cat /proc/version`, 这样查到的是宿主机的系统.
`uname -a`查到的是容器的相关信息.

## 保存docker容器的修改

`--privileged=true`是严格模式启动容器, 需要修改容器文件, 在启动容器时启用
`docker ps`查看`容器id`, `docker commit id name:version`


## 删除仓库中上传的镜像

查看仓库中镜像信息
`curl -X GET http://docker.my.com:port/v2/_catalog`


查看`Etag`, 

~~需要使用特殊的头`Accept: application/vnd.docker.distribution.manifest.v2+json`~~

`curl -I -X HEAD http://docker.my.com:port/v2/镜像名/manifests/版本号`

删除
`curl -X DELETE http://docker.my.com:port/v2/镜像名/manifests/Etag的值`


### docker无法启动的问题

一般`docker`起不来是因为`selinux`的状态问题.

`docker`启动时报错`Error starting daemon: SELinux is not supported with the overlay2 graph driver on this kernel. Either boot into a newer kernel or disable selinux in docker (--selinux-enabled=false)`

修改`sudo vim /etc/sysconfig/docker`, 中的`--selinux-enabled`为`--selinux-enabled=false`

如果需要, ~~可以修改`/etc/selinux/config`中`SELINUX`的值为`disabled`.~~, 不强求

## docker启动容器报WARNING: IPv4 forwarding is disabled. Networking will not work.

编辑系统文件`sudo vim /usr/lib/sysctl.d/00-system.conf`, 在最后追加`net.ipv4.ip_forward=1`, 
重启`sudo systemctl restart network`

