---
title: CentOS 7编译安装Redis
copyright: true
date: 2018-07-20 14:29:46
tags:
  - CentOS 7
  - Redis
  - GCC
  - GCC-C++
  - cmake
categories:
---

# CentOS 7编译安装Redis

## 安装编译相关依赖

 - CentOS/Fedora
    `yum install gcc gcc-c++ cmake tcl`

## 下载Redis源码包

`curl -o redis-4.0.10.tar.gz http://download.redis.io/releases/redis-4.0.10.tar.gz`

## 解压Redis源码包

`tar -xzvf redis-4.0.10.tar.gz`

## 编译

具体过程:
```
# 进入解压目录, 默认在当前目录
cd redis-4.0.10

# 执行make
make

# 安装, 指定目录
make PREFIX=/usr/local/redis install
```

## Redis配置

```
# 将配置文件放在/usr/local/redis/etc/下面, 放在/etc/redis/下也可以
mkdir /usr/local/redis/etc/

# 将解压目录中的redis.conf拷贝到/usr/local/redis/etc/下面
cp redis.conf /usr/local/redis/etc/

# 进入redis的执行文件目录
cd /usr/local/redis/bin/

# 将redis-benchmark, redis-cli, redis-server复制到/usr/bin
cp redis-benchmark redis-cli redis-server /usr/bin/
```

## 调整内存分配使用方式, 并使其生效:
```
# 此参数可用的值为0,1,2 
# 0.表示当用户空间请求更多的内存时，内核尝试估算出可用的内存 
# 1.表示内核允许超量使用内存直到内存用完为止 
# 2.表示整个内存地址空间不能超过swap+(vm.overcommit_ratio)%的RAM值 

echo "vm.overcommit_memory=1" >> /etc/sysctl.conf
sysctl -p
```

## 修改Redis配置和Redis加固

### Redis配置

编辑配置文件`vim /usr/local/redis/etc/redis.conf`, 将其中的对应内容修改为:
```
# redis以守护进程的方式运行, no表示不以守护进程的方式运行(会占用一个终端)
daemonize yes

# 密码
requirepass password

# 绑定ip
# bind 127.0.0.1

# 端口
port 7480

# 数据目录
dir /data/redis

# 客户端闲置多长时间后断开连接，默认为0关闭此功能
timeout 300

# 设置redis日志级别，默认级别：notice
loglevel verbose

# 设置日志文件的输出方式,如果以守护进程的方式运行redis 默认:""
# 并且日志输出设置为stdout,那么日志信息就输出到/dev/null里面去了
logfile stdout
```

### Redis加固

 - 绑定127.0.0.1
如果只是本地通信, 就确保监听在本地, 修改配置文件的`bind 127.0.0.1`

 - 设置防火墙
如果需要其他机器访问，或者设置了slave模式，那就记得加上相应的防火墙设置，命令如下:
`iptables -A INPUT -s x.x.x.x -p tcp --dport 6379 -j ACCEPT`

 - 认证
添加密码, 修改配置文件`requirepass asdlkashdkj123123`

 - 低权限账户
创建一个redis用户, 使用这个用户执行`redis-server`
```
useradd -M -s /sbin/nologin redis
setsid sudo -u redis /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf'
```

## Redis启动脚本

 - `CentOS 6`的`service`服务

```
cp /root/redis-4.0.10/utils/redis_init_script  /etc/rc.d/init.d/redis
```

在文件的开头`#!`的下一行添加`vim /etc/rc.d/init.d/redis`:
```
# chkconfig: 2345 90 10
# description: Redis is a persistent key-value database
```

如果添加了密码, 请添加`-a password`在`$CLIEXEC -a password -p $REDISPORT shutdown`中.

再在`stop`的`;;`后添加:
```
restart)
    $0 stop
    $0 start
    ;;
status)
    PID=$(cat $PIDFILE)
    if [ ! -x /proc/${PID} ]
    then
        echo 'Redis is not running'
    else
        echo "Redis is running ($PID)"
    fi
    ;;
```

例如:
```
#!/bin/sh
#
# chkconfig: 2345 90 10
# description: Redis is a persistent key-value database
# Simple Redis init.d script conceived to work on Linux systems
# as it does use of the /proc filesystem.

### BEGIN INIT INFO
# Provides:     redis_7480
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    Redis data structure server
# Description:          Redis data structure server. See https://redis.io
### END INIT INFO

REDISPORT=7480
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/usr/local/redis/etc/redis.conf"

case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, Process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF
                printf "Redis \033[1;32;32mStarted...\033[0m\n"
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, Process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $CLIEXEC -a password -p $REDISPORT shutdown >> /dev/null 2>&1
                while [ -x /proc/${PID} ]
                do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                printf "Redis \033[1;32;32mStopped...\033[0m\n"
        fi
        ;;
    restart)
        $0 stop
        $0 start
        ;;
    status)
        if [ ! -f $PIDFILE ]
        then
            echo "$PIDFILE does not exist, Process is not running"
            exit 6379
        fi
        PID=$(cat $PIDFILE)
        if [ ! -x /proc/${PID} ]
        then
            printf "Redis is \033[1;31;31mnot running\033[0m\n"
        else
            printf "Redis is \033[1;32;32mrunning\033[0m ($PID)\n"
        fi
        ;;
    *)
        echo "Please use start or stop as first argument"
        ;;
esac
```

```
# 如果没报错的话，说明成功，可以用
chkconfig --add redis

# 启动redis服务器
service redis start
# 开机自启
chkconfig redis on
# 查看是否启动成功
ps -ef | grep redis
```

 - `CentOS 7`的`systemd`服务

在`/usr/lib/systemd/system`目录下创建`redis.service`文件, 权限为`root.root 755`, 输入以下内容, 将`ExecStart`后面的执行路径修改为自己的路径:
```
[Unit]
Description=Redis persistent key-value database
After=network.target

[Service]
ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf --daemonize yes
ExecStop=/usr/local/redis/bin/redis-cli -a password -p 6379 shutdown
User=redis
Group=redis
RuntimeDirectory=redis
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
```

然后创建`/usr/libexec/redis-shutdown`文件, 输入以下内容, 记得将`REDIS_CLI`和`CONFIG_FILE`修改为自己的路径:
```
#!/bin/bash
#
# Wrapper to close properly redis and sentinel
test x"$REDIS_DEBUG" != x && set -x

REDIS_CLI=/usr/local/redis/bin/redis-cli

# Retrieve service name
SERVICE_NAME="$1"
if [ -z "$SERVICE_NAME" ]; then
   SERVICE_NAME=redis
fi

# Get the proper config file based on service name
CONFIG_FILE="/usr/local/redis/etc/$SERVICE_NAME.conf"

# Use awk to retrieve host, port from config file
HOST=`awk '/^[[:blank:]]*bind/ { print $2 }' $CONFIG_FILE | tail -n1`
PORT=`awk '/^[[:blank:]]*port/ { print $2 }' $CONFIG_FILE | tail -n1`
PASS=`awk '/^[[:blank:]]*requirepass/ { print $2 }' $CONFIG_FILE | tail -n1`
SOCK=`awk '/^[[:blank:]]*unixsocket\s/ { print $2 }' $CONFIG_FILE | tail -n1`

# Just in case, use default host, port
HOST=${HOST:-127.0.0.1}
if [ "$SERVICE_NAME" = redis ]; then
    PORT=${PORT:-6379}
else
    PORT=${PORT:-26739}
fi

# Setup additional parameters
# e.g password-protected redis instances
[ -z "$PASS"  ] || ADDITIONAL_PARAMS="-a $PASS"

# shutdown the service properly
if [ -e "$SOCK" ] ; then
    $REDIS_CLI -s $SOCK $ADDITIONAL_PARAMS shutdown
else
    $REDIS_CLI -h $HOST -p $PORT $ADDITIONAL_PARAMS shutdown
fi
```

修改`/usr/libexec/redis-shutdown`权限, `chmod 755 /usr/libexec/redis-shutdown`

重新载入`systemd`, 扫描新的或有变动的`service`, `systemctl daemon-reload`

开机自启, `systemctl enable redis`
