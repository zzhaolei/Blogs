---
title: PyCharm远程开发和调试
copyright: true
date: 2018-05-08 16:20:08
tags:
  - PyCharm
  - SFTP
categories:
---

# PyCharm远程调试

## 配置远程Python解释器

选择`File-->Settings-->Project: xxxx-->Project Interpreter`, (`xxxx`是项目根目录), 然后在右边, 点击小齿轮设置, 点击"Add Remote", 勾选`SSH Credentials`, 配置相关信息:
```
Host: IP
Port: 22
User name: test
Auth type: password # (Key, OpenSSH)
Password: *****
Project Interpreter path: /usr/bin/python
```
如果`Project Interpreter path`是使用的虚拟环境, 那么填写虚拟环境路径下的`Python`, 如`/home/user/.envs/dev/bin/python`. 同时选择`Python版本`.

## 配置远程服务器

### 配置连接
在工具栏点击`Tools-->Deployment-->Configuration...`, 然后在新的窗口的左上角点击`+`号, 新建一个连接.

输入新的连接的名称`Name`和类型`Type`(`FTP`、`SFTP`、`FTPS`、`In place`、`Local or mounted folder`), 例如:
```
Name: VirtualBox
Type: SFTP
```

点击`OK`, 在连接详细配置中的`Upload/download project files`下, 配置:
```
SFTP host: 127.0.0.1
Port: 6666
Root path: /home/user/workspace
User name: user
Auth type: password
Password: ******
```

`SFTP host`是远程服务器的`IP`地址. 

`Port`是`SSH`的端口. 

`Root path`是项目文件上传的路径, 必须真实存在, `PyCharm`不会自动创建, 可以在配置好`User name`和`Password`后, 点击`Root path`输入框后的`...`, 手动选择远程服务器的目录.

`Auth type`是验证类型, 有`password`、`Key pair(OpenSSH or PuTTY)`、`OpenSSH config and authentication agent`.

推荐使用`Key pair(OpenSSH or PuTTY)`, 使用`Key`需要先配置一些相关的信息, `Windows`上需要安装`Git`, 以便使用自带`openssh`的一些功能, 下载[Git](https://git-scm.com/)客户端, 双击安装. 这个网上有教程.

在`Windows`上和`Linux`上`Git`会自动处理换行符的问题, 但是这个在实际开发中会出现问题, 所以, 在`Windows`上使用`提交时转换为LF，检出时不转换`功能, 统一使用Unix换行符(也可以在类Unix上启用这个功能, 将拉取的没有改变的换行符更换). 在打开安装好的`Git Bash`, 输入:
```
git config --global core.autocrlf input
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

生成`Key`, `ssh-keygen -t rsa -C "your@email.com"`,

配置`.gitconfig`, 
```
git config --global user.name "username"
git config --global user.email your@email.com
```

将本地生成的`Key`, 上传到远程服务器, 实现免密码登录, `ssh-copy-id -p 6666 -i user@127.0.0.1`

如果`ssh`的端口是`22`, 那么就需要指定`-p`, `-p`后跟着的是`ssh`修改的`端口`, `user@127.0.0.1`是远程服务器的`用户名`和`IP`.


配置完成后, 可以点击`SFTP host`后的`Test SFTP connection...`测试连接是否成功, 如果显示`Successfully`, 成功.

### 配置映射

在连接配置完成后, 还有一步重要的工作, 配置`Mappings`

点击当前窗口的顶部的`Mappings`标签, 

`Local path`是远程开发的项目的根目录

`Deployment path on server 'VirtualBox'`是相对于`Root path`路径的, 存放当前项目的根目录下的所有文件, 默认是`/`, 可以在`Root path`路径中新建一个目录, 分别存放不同的项目.

`Web path on server 'VirtualBox'`不需要配置, 默认是`/`

例如:
```
Local path:                                 C:\Users\user\Desktop\project
Deployment path on server 'VirtualBox':     /project
Web path on server 'VirtualBox':            /
```

### 同步文件

配置好全部信息后, 需要执行文件, 将本地项目的所有文件和修改, 上传到远程服务器.

`Tools-->Deployment`, 

`Upload to VirtualBox`: 如果当前选中的是一个文件, 那么上传这个文件到远程服务器. 如果选中的是`根目录`, 那么将整个项目上传到远程服务器.
`Upload to`: 如果有多个远程配置, 那么这个可以选择上传到具体的远程服务器.

`Download from VirtualBox`: 如果本地选择的是一个文件, 那么就会从远程复制文件到本地, 会询问是否覆盖本地文件(无论文件是否修改). 如果本地选择的是目录, 那么会一个文件一个文件的询问是否覆盖.
`Download from `: 选择远程服务器.

`Compare with Deployed Version on VirtualBox`: 对选中的文件和远程服务器进行比较.
`Compare with Deployed Version on `: 选择远程服务器..

`Sync with Deployed on VirtualBox`: 与远程服务器同步
`Sync with Deployed on `: 选择远程服务器.
