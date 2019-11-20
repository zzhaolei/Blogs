---
title: Windows鼠标右键菜单添加SublimeText打开选项
copyright: true
date: 2018-05-06 10:04:28
tags:
  - Windows
  - SublimeText
categories:
---

# Windows上将使用`SublimeText`打开文件的选项添加到鼠标右键菜单。

新建`reg`后缀的注册表文件，编辑添加内容
```
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\*\shell\SublimeText3]
@="Open To Sublime Text 3"
"Icon"="C:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"
[HKEY_CLASSES_ROOT\*\shell\SublimeText3\command]
@="C:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"
[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3]
@="Open To Sublime Text 3"
"Icon"="C:\\Program Files\\Sublime Text 3\\sublime_text.exe,0"
[HKEY_CLASSES_ROOT\Directory\shell\SublimeText3\command]
@="C:\\Program Files\\Sublime Text 3\\sublime_text.exe %1"
```

`@=`后面的`Open To Sublime Text 3`是右键后显示名字,
`C:\\...`是`Sublime Text 3`的安装路径。

# 删除右键内容

在`regedit`中删除`HKEY_CLASSES_ROOT\*\shell\SublimeText3`
