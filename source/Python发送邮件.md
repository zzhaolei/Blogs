---
title: Python发送邮件
copyright: true
date: 2018-07-13 18:06:46
tags:
  - Email
  - smtplib
  - email
  - Python
categories:
---

# Python发送邮件

日志监控功能需要当关键字出现指定次数时，向客户发送邮件，进行提醒.

## 实现

一般的邮件服务提供商都需要密码，这个密码不是账户密码，而是专门生成的客户端密码.

```
#!/usr/bin/python
# -*- coding:utf-8 -*-

import smtplib
from email.mime.text import MIMEText
from email.header import Header


#设置发送服务器
smtp_send = "smtp.exmail.qq.com"
# 用户名和客户端授权密码
username = "zhaolei@qq.com"
passwd = "xxxxxxxxxxx"

# 发送邮箱
sender = 'zhaolei@qq.com'
# 接收邮箱, 列表
receivers = ['zhaolei@qq.com']

# 文本内容, 文本格式, 编码
message = MIMEText("出错了！！！！！", "plain", "utf-8")
# 发件人, 名称
message["From"] = Header("zhaolei", "utf-8")
# 接收人, 名称
message["To"] =  Header("测试", "utf-8")

# 子标题
subject = "日志错误警报!"
message["Subject"] = Header(subject, "utf-8")

try:
    smtpObj = smtplib.SMTP_SSL(smtp_send, 465)
    smtpObj.login(username, passwd)
    smtpObj.sendmail(sender, receivers, message.as_string())
    smtpObj.quit()
    print "邮件发送成功"
except smtplib.SMTPException:
    print "Error: 无法发送邮件"

```
