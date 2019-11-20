---
title: Windows 10使用Tesseract-OCR遇到WindowsError
copyright: true
date: 2018-05-06 10:02:12
tags:
  - Windows 10
categories:
---

# Windows 10使用Tesseract-OCR出现WindowsError: [Error 2]

Tesseract-OCR安装时默认安装在x86的目录下，手动添加环境变量`此电脑-->属性-->高级系统设置-->环境变量`,点击系统变量里的Path, 点击编辑，在`编辑环境变量`界面中点击新建，输入安装目录，类似`C:\Program Files (x86)\Tesseract-OCR\`这种，一定要在最后加一个反斜线，不然cmd没法识别。确定保存。可以再cmd中测试是否成功。
