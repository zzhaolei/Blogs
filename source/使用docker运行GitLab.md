---
title: 使用docker运行GitLab
copyright: true
date: 2018-05-06 10:07:16
tags:
  - docker
  - GitLab
categories:
---

从`docker`镜像拉取代码,`docker pull gitlab/gitlab-ce:latest`.

创建`/srv/gitlab`目录`sudo mkdir /srv/gitlab`

启动`GitLab CE`容器,并发布访问`SSH`,`HTTP`,`HTTPS`的端口.
所有`GitLab`数据将存储在`/srv/gitlab`目录.
如果只在本机测试,把`hostname`改为本机IP地址,（如果是在虚拟机中启动，主机测试，把IP地址设置为虚拟机ip地址）.如果让外部系统访问则改为外网IP.
`docker run --detach --hostname gitlab.example.com --publish 443:443 --publish 80:80 --publish 22:22 --name gitlab --restart always --volume /srv/gitlab/config:/etc/gitlab --volume /srv/gitlab/logs:/var/log/gitlab --volume /srv/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:latest`

如果开启了`selinux`,运行下面的代码：
`docker run --detach --hostname gitlab.example.com --publish 443:443 --publish 80:80 --publish 22:22 --name gitlab --restart always --volume /srv/gitlab/config:/etc/gitlab:Z --volume /srv/gitlab/logs:/var/log/gitlab:Z --volume /srv/gitlab/data:/var/opt/gitlab:Z gitlab/gitlab-ce:latest`

`--restart always`: 系统重启后，容器启动

确保`docker`有足够的权限在挂在的卷中创建文件.

边间GitLab配置文件，可以打开一个shell`sudo docker exec -it gitlab /bin/bash`,或者直接编辑文件`sudo docker exec -it gitlab vi /etc/gitlab/gitlab.rb`



在浏览器中输入gitlab所在服务器的IP地址,初次使用需要修改密码.

生成key`ssh-keygen -t rsa -C "email"`，将key添加到gitlab上。

配置`.ssh`目录和文件的权限
`chmod 755 ~/.ssh/    
 chmod 600 ~/.ssh/id_rsa ~/.ssh/id_rsa.pub     
 chmod 644 ~/.ssh/known_hosts`
权限过高会出错。

免密码登录
//将文件拷贝至远程服务器
`scp ~/.ssh/id_rsa.pub root@<remote_ip>:pub_key`

//将内容追加到authorized_keys文件中， 不过要登录远程服务器来执行这条命令
`cat ~/pub_key >>~/.ssh/authorized_keys`

编辑`/etc/ssh/sshd_config`文件，将`PubkeyAuthentication yes`行和`AuthorizedKeysFile      .ssh/authorized_keys`的注释删掉.重启`sshd`服务.

