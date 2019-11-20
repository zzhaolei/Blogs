---
title: Mac上使用brew update会卡住的问题
copyright: true
date: 2018-12-18 10:48:24
tags:
  - Mac
  - brew
categories:
---

# Mac上使用brew update会卡住的问题
`brew`默认的源是`Github`，会非常慢，建议换为国内的源。推荐`中科大的镜像源`，比较全面.

## 解决方案

### Homebrew
`Homebrew`源代码仓库

 - 替换源地址:
    ```
    cd "$(brew --repo)"
    git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
    ```

 - 重置为官方地址:
    ```
    cd "$(brew --repo)"
    git remote set-url origin https://github.com/Homebrew/brew.git
    ```

### Homebrew Bottles
`Homebrew`预编译二进制软件包

在运行`brew`, 前设置环境变量`HOMEBREW_BOTTLE_DOMAIN`，值为`https://mirrors.ustc.edu.cn/homebrew-bottles`

 - 替换源地址:
    - bash:
        ```
        echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
        source ~/.bash_profile
        ```

    - zsh:
        ```
        echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc
        source ~/.zshrc
        ```

### Homebrew Core
`Homebrew`核心软件仓库

 - 替换源地址:
    ```
    cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
    git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
    ```

 - 重置为官方地址:
    ```
    cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
    git remote set-url origin https://github.com/Homebrew/homebrew-core
    ```

### Homebrew Cask
`Homebrew cask`软件仓库，提供`macOS`应用和大型二进制文件

 - 替换源地址:
    ```
    cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
    git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
    ```

 - 重置为官方地址:
    ```
    cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
    git remote set-url origin https://github.com/Homebrew/homebrew-cask
    ```
