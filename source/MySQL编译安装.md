---
title: MySQL编译安装
copyright: true
date: 2018-07-06 16:51:48
tags:
  - CentOS 7
  - MySQL
  - GCC
  - GCC-C++
  - cmake
categories:
---

# MySQL编译和安装

在`CentOS7`中编译安装`MySQL 5.7.21`.

## 依赖和源码包

安装相关的依赖:
```
yum install gcc gcc-c++ ncurses ncurses-devel cmake bison -y
```

下载`MySQL 5.7.21`源码包和依赖`boost`, `MySQL 5.7.21`依赖`boost 1.59.0`:
```
curl -o boost_1_59_0.tar.gz https://jaist.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz
curl -o mysql-5.7.21.tar.gz http://cdn.mysql.com/Downloads/MySQL-5.7/mysql-5.7.21.tar.gz
```

解压下载的包:
```
# 进入下载的路径

# 解压到/usr/local/目录
tar -xzvf boost_1_59_0.tar.gz -C /usr/local/
# 解压到当前目录
tar -xzvf mysql-5.7.21.tar.gz
```

## 创建用户和组
创建`MySQL`用户和组, 并且用户不能登陆:
```
groupadd -r mysql && useradd -r -g mysql -s /sbin/nologin -M mysql
```

## 创建相关的目录
 - 创建数据目录
    ```
    mkdir -p /data/mysql && chown mysql.mysql /data/mysql
    ```
 - 创建log目录
    ```
    mkdir -p /var/log/mysql && chown mysql.mysql /var/log/mysql
    ```
 - 创建UNIX SOCK目录
    ```
    mkdir -p /var/lib/mysql && chown mysql.mysql /var/lib/mysql
    ```
 - 创建PID目录
    ```
    mkdir -p /var/run/mysqld && chown mysql.mysql /var/run/mysqld
    ```

## 预编译
使用各种参数, 预编译源代码. 进入解压的`MySQL`源码目录, 执行以下命令:
```
cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/data/mysql \
-DSYSCONFDIR=/etc \
-DMYSQL_UNIX_ADDR=/var/lib/mysql/mysqld.sock \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DENABLED_LOCAL_INFILE=1 \
-DENABLED_PROFILING=1 \
-DMYSQL_TCP_PORT=3306 \
-DWITH_DEBUG=0 \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=/usr/local/boost_1_59_0
```

```
DCMAKE_INSTALL_PREFIX=/usr/local/mysql      :安装路径
DMYSQL_DATADIR=/data/mysql                  :数据文件存放位置
DSYSCONFDIR=/etc                            :my.cnf路径
DMYSQL_UNIX_ADDR=/var/lib/mysql/mysqld.sock :连接数据库socket路径
DEXTRA_CHARSETS=all                         :安装所有的字符集
DDEFAULT_CHARSET=utf8                       :默认字符
DDEFAULT_COLLATION=utf8_general_ci          :排序集
DWITH_MYISAM_STORAGE_ENGINE=1               :支持MyIASM引擎
DWITH_INNOBASE_STORAGE_ENGINE=1             :支持InnoDB引擎
DWITH_PARTITION_STORAGE_ENGINE=1            :安装支持数据库分区
DENABLED_LOCAL_INFILE=1                     :允许从本地导入数据
DENABLED_PROFILING=1                        :
DMYSQL_TCP_PORT=3306                        :端口
DWITH_DEBUG=0                               :
DDOWNLOAD_BOOST=1                           :允许下载
DWITH_BOOST=/usr/local/boost_1_59_0         :本地boost路径
```

## 编译安装
预编译完成后, 执行下面的命令编译, 安装:
```
# 指定CPU数量编译
make -j `grep processor /proc/cpuinfo | wc -l` && make install
```

## 添加开机自启
对目录修改权限, 添加`service/systemd`服务:
```
chown -R mysql:mysql /usr/local/mysql
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld
chmod +x /etc/init.d/mysqld

# 开机自启
chkconfig --add mysqld
chkconfig mysqld on
```

## 环境变量
将`/usr/local/mysql/bin`添加进入`环境变量`, 或者直接使用`软链接`的方式链到`/usr/local/bin`下:
```
# 添加到环境变量
echo "" >> /etc/bashrc
echo "export PATH=/usr/local/mysql/bin:$PATH" >> /etc/bashrc
echo "" >> /etc/bashrc
source ~/.bashrc

# 使用软链接
ln -s /usr/local/mysql/bin/* /usr/local/bin/
```

## 初始化数据库
以上都完成后, 还不能启动MySQL, 如果非要启动, 会报错. 需要初始化数据库:
```
/usr/local/mysql/bin/mysqld  --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/data/mysql
```
```
--user      :指定用户
--basedir   :mysql所在目录
--datadir   :mysql数据库和表所在的目录, 以及PID文件
```

初始化后, 会有一行提示, 冒号后面的是初始密码`root@localhost: password`:
```
A temporary password is generated for root@localhost: xKefZvib13)5
```

## 启动服务
以上都配置完成, 就可以启动服务了:
```
# 使用service
service mysqld start

# 使用systemd
systemctl daemon-reload
systemctl start mysqld
```

## 修改密码

将初始密码修改成自己的密码, 直接在`shell`中输入命令: `mysqladmin -uroot -p'old_pass' password 'new_pass'`

## 配置文件
默认`MySQL`不需要配置文件, 编译时已经配置好了, 但是也可以使用配置文件, 指定`log`的位置, 编辑`vim /etc/my.cnf`, 将以下内容添加到文件中:
```
[mysqld]
datadir=/data/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/var/log/mysql/mysqld.log
# pid-file=/var/run/mariadb/mariadb.pid

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

```
