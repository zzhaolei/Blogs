---
title: Git相关使用
copyright: true
date: 2018-05-09 18:06:43
tags:
  - Git
categories:
---

# Git工具的相关使用

## Git换行符

`Git`会自动处理换行符的问题, 但是这个在实际开发中会出现问题, 所以, 在`Windows`上使用`提交时转换为LF，检出时不转换`功能, 统一使用Unix换行符(也可以在类Unix上启用这个功能, 将拉取的没有改变的换行符更换). 

`Windows`打开安装好的`Git Bash`, `类Unix`直接输入:
```
git config --global core.autocrlf input
git config --global core.safecrlf warn
```

含义：
```
AutoCRLF
# 提交时转换为LF，检出时转换为CRLF
git config --global core.autocrlf true

# 提交时转换为LF，检出时不转换
git config --global core.autocrlf input

# 提交检出均不转换
git config --global core.autocrlf false

SafeCRLF
# 拒绝提交包含混合换行符的文件
git config --global core.safecrlf true

# 允许提交包含混合换行符的文件
git config --global core.safecrlf false

# 提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn
```

## Git创建分支、 删除本地和远程分支、 创建tag、 修改tag名称

 - 创建分支: `git checkout -b develop`

 - 删除本地分支: `git branch -d develop`

 - 删除远程分支: `git push origin :develop`

 - 创建`tag`: `git tag -a v1.0.0 -m "message"`

 - 推送`tag`: `git push origin --tags`

 - 修改`tag`名称: 
    ```
    git tag new old

    git tag -d old

    git push origin :refs/tags/old

    git push --tags
    ```

 - 修改`commit`的信息:
    如果这是你最近一次提交并且没有`push`到远程分支, 可用以下命令直接修改:
    `git commit --amend -m "your new message"`
