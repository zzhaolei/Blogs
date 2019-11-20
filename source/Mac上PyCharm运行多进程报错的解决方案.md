---
title: Mac上PyCharm运行多进程报错的解决方案
copyright: true
date: 2018-12-17 11:03:54
tags:
  - Mac
  - PyChamr
  - 多进程
categories:
---

# Mac上PyCharm运行多进程报错的解决方案
### 运行时报错
```
may have been in progress in another thread when fork() was called. We cannot safely call it or ignore it in the fork() child process. Crashing instead. Set a breakpoint on objc_initializeAfterForkError to debug.
```

### 解决方案
添加环境变量: `点击窗口上的Run`->`Edit Configurations...`->`Environment variables`->`点击输入栏后的文件夹图标`,

添加内容，`key`: `OBJC_DISABLE_INITIALIZE_FORK_SAFETY`, `value`: `YES`.

完整示例, `OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES`
