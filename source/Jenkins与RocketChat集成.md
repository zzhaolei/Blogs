---
title: Jenkins与RocketChat集成
copyright: true
date: 2018-05-17 17:59:54
tags:
  - Jenkins
  - RocketChat
categories:
---

# Jenkins与RocketChat集成

## 在Jenkins中安装插件RocketChat Notifier

## 配置信息

点击`Jenkins`左侧的`系统管理-->系统设置`, 找到`Global RocketChat Notifier Settings`.

配置`Rocket Server URL`, 是`URL:PORT`的类型, 例: `http://chat.xxxx.com:80`.

配置`Login Username`和`Login password`, 是`RocketChat`的账号密码.

`Channel`, 发送的频道.

`Build Server URL`, 构建的服务器和端口, `http://192.168.0.1:8080`

点击`Test Connection`, `Success`表示配置成功.


