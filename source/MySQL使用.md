---
title: MySQL使用
copyright: true
date: 2018-05-26 17:56:04
tags:
  - MySQL
categories:
---

# MySQL使用

## 创建用户
 - 登陆MySQL
    `mysql -uroot -p[可选，直接写密码]`

 - 创建本地访问的用户
    `create user 'test'@'localhost' identified by '123456';`

 - 创建远程访问用户
    `create user 'test'@'%' identified by '123456';`

 - 刷新授权
    `flush privileges;`

`test`为用户名, `123456`是用户密码, 将这两者替换为自己的用户名密码, 如果密码不写，那么登陆不需要密码.

## 授权

`grant all privileges on *.* to test@'localhost' with grant option;`

`all privileges`是将所有的权限如`select, delete`等, 赋予用户. 可以这样写`grant select, insert on *.* to 'lei'@'%';`

`*`第一个代表指定的`数据库`, 第二个表示指定的`表`.

`test`是用户名.

`localhost`是本地访问用户, 如果是远程访问用户将其改为`%`

## 删除用户

`DROP USER 'username'@'localhost';`

## 更新用户密码

`SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');`

## 撤销用户权限

`REVOKE privilege ON databasename.tablename FROM 'username'@'host';`

`privilege, databasename, tablename` 同授权部分.

## 错误解决

如果连接时报错`Authentication plugin 'caching_sha2_password' cannot be loaded`, 是因为`8.0`修改身份验证插件, `5.7及其之前的`版本是`mysql_native_password`, 登陆`MySQL`的`root`账户执行:

`alter user 'zhaolei'@'%' identified with mysql_native_password by "zhaolei";`

# shell中操作MySQL

## 修改密码

直接在`bash`中输入`mysqladmin -uroot -p'old_pass' password 'new_pass'`命令就可以直接修改密码, 会显示`warning`可以忽略.

## 执行MySQL命令

直接在`bash`中输入`mysql -uroot -p'password' -e 'flush privileges;'`

## 导入sql文件

如果文件中包含数据库和表结构以及数据, 那么可以直接使用`mysql -uroot -p'password' < test.sql`, 否则可以使用`mysql -uroot -p'password' db_name < test.sql`
