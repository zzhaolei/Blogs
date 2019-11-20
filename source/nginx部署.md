---
title: nginx部署
copyright: true
date: 2018-05-06 09:53:41
tags:
  - nginx
categories:
---

# nginx部署

`CentOS 7`安装`nginx`, `sudo yum install nginx`

设置开机自启
```
sudo systemctl start nginx
sudo systemctl enable nginx
```

访问主机的`ip`, 或者域名

编辑`sudo vim /etc/nginx/nginx.conf`, 在`http`中的`{}`中添加:
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
    access_log logs/pypi.log main;
}
```

修改`user xxx`为`path`文件夹的用户和组名`user name group`.

使用`sudo nginx -t`查看配置文件是否正确
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

