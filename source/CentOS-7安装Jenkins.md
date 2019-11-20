---
title: CentOS 7安装Jenkins
copyright: true
date: 2018-05-06 10:07:56
tags:
  - Docker
  - Jenkins
  - CentOS 7
categories:
---

# 使用Docker安装Jenkins

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

从服务器拉取`jenkins/jenkins:lts`镜像，`docker pull jenkins/jenkins:lts`

`home`目录创建jenkins文件夹`mkdir jenkins`

## 启动容器
`docker run -u root --rm -d -p 8080:8080 -p 50000:50000 -v $HOME/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts`

系统重启，容器启动：
`docker run -u root -d -p 8080:8080 -p 50000:50000 --name jenkins --restart always -v $HOME/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts`

--rm : 退出并删除容器

在浏览器中输入`本机ip:8080`，即可访问。会提示密码路径，使用命令`sudo cat $HOME/jenkins/secrets/initialAdminPassword`查看密码.
在浏览器中输入密码，选择插件，创建账户。

有的插件需要以手动下载和上传的方式安装。从这个网站`http://updates.jenkins-ci.org/download/plugins/`查找需要手动安装的包文件夹，下载。

## 删除所有容器(除了正在运行的)
`docker rm $(sudo docker ps -a -q)`

## 停止正在运行的所有容器
`docker stop $(sudo docker ps -q)`

## docker使用`apt-get`进行安装和其他操作，默认就是root权限。

## 关于报错
如果系统重启后，使用`docker ps -a`报以下错误
`Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`

一般是因为`daemon.json`配置文件出问题了。
删除里面的配置，重启，再添加源，重启。
也可能是selinux没有关闭的原因。


# 使用安装的包的方式安装Jenkins

## 安装Jenkins
 - 安装`Jenkins`源文件
    `curl -o /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo`,

 - 导入秘钥
    `rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key`

 - 安装Jenkins
    `yum install jenkins` 

## 安装依赖

`Jenkins`依赖`JDK`[官网](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

下载`jdk-8u181`, 安装

# 使用Jenkins

启动`jenkins`服务, `systemctl start jenkins`, 直接在浏览器只能够访问`http://localhost:8080`, 默认端口是`8080`

1. 会进入`Unlock Jenkins`界面，里面会有高亮显示的路径，这个路径里面保存的是初始化`admin`的密码, `/var/lib/jenkins/secrets/initialAdminPassword`, 在终端中查看密码`cat /var/lib/jenkins/secrets/initialAdminPassword`, 将密码拷贝进页面的输入框内，点击`Continue`.

2. 安装插件，这里使用的是默认的插件，可以自定义需要安装的插件。

3. 注册账号，使用自己心仪的用户名和密码，以及邮箱

4. 开始使用

## Jenkins插件换源

`Jenkins`插件下载和更新的会非常慢，更换为国内的源以提升速度。

进入`jenkins`的`系统管理-->插件管理-->高级`, 将`升级站点`的`URL`替换为国内的源, 这里使用的是清华大学的镜像`https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json`

## Jenkins一般需要的插件

 - GitLab相关
`GitLab, GitLab hook, Gitlab Authentication, Build Authorization Token Root, Generic Webhook Trigger`

 - SSH
`Publish over SSH, SSH`

 - 邮件
`Email Extension`
