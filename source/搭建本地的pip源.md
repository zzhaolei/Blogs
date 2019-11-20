---
title: 搭建本地的pip源
copyright: true
date: 2018-05-06 10:05:21
tags:
  - pip
categories:
---

# 搭建本地的pip源

开发环境部署机器的时候, 每次从网上下载pip包会很慢, 将需要的包和相关依赖下载到本地, 搭建一个本地源服务器。

基本都是安装多个包, 推荐使用文件的方式, 
文件内容格式, 可以指定版本`package==1.0.0`, 也可以不指定`package`, 每个包名在一行:
```
package==1.0.0
package==1.0.0
package==1.0.0
```

## 安装`pip2pi`工具

安装`pip install pip2pi`, `pip2pi`中包含很多相关的命令。

## 在指定目录下创建存放源文件的目录
`mkdir ~/pypi`

## 下载包
下载单独包`pip2tgz path package==1.0`, 可以指定版本, 默认是最新的包

批量下载`pip2tgz path -r requirements.txt`

## 建立索引

`dir2pi path`, 会创建`simple`文件夹

## 更新索引

多个包: `pip2acmeco -r requirements.txt`

单个包: `pip2acmeco package==1.0.0`

## 搭建服务器

使用`nginx`, `sudo yum install nginx`, 修改配置文件`sudo vim /etc/nginx/nginx.conf`
```
server {
    listen 80;
    server_name ip或域名;
    root path;
    location /{
        autoindex on;
        autoindex_exact_size off; #显示文件的大小
        autoindex_localtime on; #显示文件时间
        #limit_rate_after 5m; #5分钟后下载速度限制为200k
        limit_rate 200k;
    }
    access_log /var/log/nginx/pypi.log main;
}
```
修改`user xxx`为`path`文件夹的用户和组名`user name group`.

使用`sudo nginx -t`查看配置文件是否正确
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

重启`nginx`服务器`sudo systemctl restart nginx`

访问`pypi/simple`报`403`错误, 查看`ps -ef | grep nginx`显示的`worker process`是否是和`pypi`文件夹的权限一致, 如果不一致, 修改`nginx.conf`, 修改`user xxx`为, `user name group`, `name`和`group`是`pypi`的文件夹用户和组.


## 使用搭建的pypi源

### 全局配置pip源
编辑`~/.pip/pip.conf`文件:
```
[global]
index-url=http://ip:port/pypi/simple
```

### 虚拟环境pip源
编辑虚拟环境中`venv/pip.conf`:
```
[global]
index-url=http://ip:port/simple
[install]
trusted-host=ip
```

示例
```
sudo yum install -y epel-release
sudo yum install -y mysql-devel
sudo yum install -y nginx

sudo pip install --upgrade pip
sudo pip2tgz /home/pypi/ -r requirements.txt
sudo dir2pi /home/pypi
sudo pip2acmeco -r requirements.txt
```

