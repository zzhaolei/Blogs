---
title: 向Rocket.Chat推送消息
copyright: true
date: 2018-07-19 15:40:57
tags:
categories:
---

# Rocket.Chat推送消息

`Rocket.Chat`是一个开源实时通讯平台, 支持`Windows`, `Mac OS`, `Linux`. 支持聊天, 文件上传, 视频通话, 语音通话功能.

## 向Rocket.Chat推送消息

以下示例可以转为别的语言的版本, 本示例使用`Linux`平台的`curl`测试, `curl`非常强大.

### 登陆

首先需要登陆`Rocket.Chat`服务器:
```
curl http://localhost:3000/api/v1/login -d "username=MyUserName&password=MyPassWord"

# 会返回一个json数据, 包含了userId和Token
{
	"status": "success",
	"data": {
		"userId": "jhsjkdhaskjdasdasd",
		"authToken": "jhsjkdhaskjdasdasdjhsjkdhaskjdasdasdjhsjkdhaskjdasdasd-"
	}
}
```

### 发送信息

使用返回的`userId`和`authToken`, 构造新的请求:
```
curl -H "X-Auth-Token: jhsjkdhaskjdasdasdjhsjkdhaskjdasdasdjhsjkdhaskjdasdasd" \
     -H "X-User-Id: jhsjkdhaskjdasdasd" \
     -H "Content-type:application/json" \
     http://localhost:3000/api/v1/chat.postMessage \
     -d '{ "channel": "#系统研发讨论", "text": "This is a test! @all" }'

# 返回, 会包含时间戳, 频道, 信息的id, 发送的用户, @的用户等信息
{
	"ts": 1531986688452,
	"channel": "#系统研发讨论",
	"message": {
		"alias": "",
		"msg": "This is a test! @all",
		"attachments": [],
		"parseUrls": true,
		"groupable": false,
		"ts": "2018-07-19T07:51:28.447Z",
		"rid": "jhasjkdhak",
		"u": {
			"_id": "kjashdashasjfgh",
			"username": "zhaolei",
			"name": "zhaolei"
		},
		"unread": true,
		"mentions": [{
			"_id": "all",
			"username": "all"
		}],
		"channels": [],
		"_updatedAt": "2018-07-19T07:51:28.448Z",
		"_id": "kjashdashasjfgh"
	},
	"success": true
}
```
