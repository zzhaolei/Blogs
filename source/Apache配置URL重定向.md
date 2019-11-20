---
title: Apache配置URL重定向
copyright: true
date: 2018-05-06 09:41:00
tags:
  - Apache
  - httpd
categories:
---

# Apache配置URL重定向

修改`/etc/httpd/conf/httpd.conf`文件的内容
```
<Directory "/var/www">
    AllowOverride None
    # Allow open access:
    Require all granted
</Directory>
```
为
```
<Directory "/var/www">
    AllowOverride All
    # Allow open access:
    Require all granted
        RewriteEngine On
        RewriteRule /task/* /index.html?$1
</Directory>
```
