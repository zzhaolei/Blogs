---
title: Mac OS iTerm2使用lrzsz上传和下载文件
copyright: true
date: 2018-08-09 11:44:44
tags:
  - Mac OS
  - iTerm2
  - lrzszs
categories:
---

# Mac OS iTerm2使用lrzsz对服务器上传和下载文件

## 安装工具

首先需要安装`iTerm2`和`homebrew`，在终端中执行(打开终端，使用搜索(`command + space`)，输入`terminal.app`, 然后回车):
```
# iTerm2
curl -o iTerm2.zip https://iterm2.com/downloads/stable/iTerm2-3_2_0.zip
# 解压
unzip iTerm2.zip

直接在Finder中双击解压后的文件, 标准的Mac软件安装流程

# homebrew， 直接在终端中执行
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# 修改为国内的软件源，速度会快很多, 这里使用的是中科大的镜像源
cd "$(brew --repo)"
git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin git://mirrors.ustc.edu.cn/homebrew-core.git
```

## 配置

 - 安装lrzsz
    `brew install lrzsz`

 - 克隆脚本
    ```
    # 克隆zmodem的iTerm2脚本
    git clone https://github.com/mmastrac/iterm2-zmodem.git

    # 进入目录
    cd iterm2-zmodem
    # 查看文件是否有可执行权限, 如果没有可执行权限，则修改权限
    chmod 755 iterm2-recv-zmodem.sh
    chmod 755 iterm2-send-zmodem.sh

    # 将这两个文件拷贝到/usr/local/bin下
    cp -f iterm2-recv-zmodem.sh /usr/local/bin/
    cp -f iterm2-send-zmodem.sh /usr/local/bin/
    ```

 - iTerm2添加Triggers
    ```
    Regular expression: \*\*B0100
        Action: Run Silent Coprocess
        Parameters: /usr/local/bin/iterm2-send-zmodem.sh
    Regular expression: \*\*B00000000000000
        Action: Run Silent Coprocess
        Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
    ```
    添加步骤: `点击状态栏的iTerm2 -> 打开Preferences -> Profiles -> 选择自己的配置文件 -> 点击右侧的Advanced标签 -> 点击Triggers的Edit - > 点击左下角的+号，根据上面的规则添加`，添加完毕点击右下角的`close`，关闭所有窗口，重启`iTerm2`.

现在可以在服务器上使用`rz`和`sz`命令上传和下载文件了，需要在服务器安装软件包`lrzsz`:
 - CentOS/Fedora
```
yum install epel-release
yum install lrzsz
```

 - Ubuntu/Debian
```
apt install lrzsz
```
