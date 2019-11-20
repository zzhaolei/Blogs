---
title: Git项目迁移
copyright: true
date: 2018-09-12 11:54:53
tags:
categories:
---

# Git项目迁移

## 代码项目迁移步骤

```
1.将原有项目重命名，old
2.新建一个项目，名字为原本的项目名称，new
3.使用特殊方式克隆代码
    # old.git为原项目重命名后的git链接
    git clone --mirror old.git

4.进入原项目目录中
    # 这种方式克隆的代码，目录名为old.git
    cd old.git

5.将老项目地址更换为新项目的git地址
    # new.git 是新项目的git链接
    git remote set-url --push origin  new.git

6.推送到远程
    git push --mirror

7.将重命名后的项目（old）删除
```

