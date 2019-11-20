---
title: Django迁移报错
date: 2018-04-27 19:32:22
tags:
  - Django
categories:
copyright: true
---

# Django迁移数据库报错

## table "xxx" already exists错误

`django`在`migrate`时报错`django migrate error: table 'xxx' already exists`错误。

 解决方案`python manage.py migrate --fake <appname>`
