---
title: CentOS 7使用ElasticSearch
copyright: true
date: 2018-07-26 14:50:44
tags:
  - CentOS
  - ElastiSearch
categories:
---

# CentOS 7使用Elasticsearch

## 安装ElasticSearch

### 下载依赖

`Elasticsearch`依赖`jdk`, 在[官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载`jdk`压缩包, 或者直接安装.

下载压缩包, 解压`tar -xzvf jdk-8u181-linux-x64.tar.gz`

添加环境变量, 编辑`vim ~/.bashrc`:
```
# 路径是下载的jdk的解压路径
JAVA_HOME=/root/jdk1.8.0_181/bin
PATH=$JAVA_HOME:$PATH
export JAVA_HOME PATH
```

或者直接使用软链的方式:
```
# /root/jdk1.8.0_181/bin/java这个路径要是一个完整的路径
ln -s /root/jdk1.8.0_181/bin/java /usr/bin
```

### 下载软件包

在[Elasticsearch](https://www.elastic.co/cn/downloads/elasticsearch)官网下载对应系统的`软件包`或者`压缩包`, 

`elasticsearch 6.x`去除了`type`类型, 性能也有所优化. 这里使用最后一个支持`type`的版本.

`curl -o elasticsearch-5.6.10.zip https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.10.zip`

解压`tar -xzvf elasticsearch-5.6.10.zip`

`Elasticsearch`不能使用`root`账户运行, 创建一个账户`useradd -M -s /sbin/nologin elasticsearch`

对文件授权`chmod -R elasticsearch.elasticsearch elasticsearch`

后台启动`Elasticsearch`, `sudo -u elasticsearch elasticsearch/bin/elasticsearch -d`.

### JVM的占用大小

`elasticsearch`查询对内存的要求比较高, 可以提高`JVM`的大小, 编辑解压文件里的`config/jvm.options`文件的字段, 修改为如下:
```
-Xms25g
-Xmx25g
```

给系统稍微留点就行, 视自己的内存而定.
