---
title: 手动安装gitlab-runner
copyright: true
date: 2018-05-06 10:08:53
tags:
  - GitLab-Runner
categories:
---

# 手动安装gitlab-runner

在终端使用命令`curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash`，输入`sudo`密码,导入指纹秘钥。速度有点慢。

安装`sudo yum install gitlab-runner`。

打开自己搭建的`GitLab`网站，点击顶栏的`Snippets`后面的小扳手，再点击左侧列表中`Overview`中的`Runners`，在打开的网页下面，可以看到`How to setup a shared Runner for a new project`行，`2是Runners设置时需要指定的URL,3是在设置是的Runners`。


## 注册`gitlab-runner`
`sudo gitlab-runner register`，

`Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):`输入域名或者服务器`ip`地址，格式为`https://gitlab.com`。和`token` `Please enter the gitlab-ci token for this runner:`。

`Please enter the gitlab-ci description for this runner:`输入`runner`描述。

`Please enter the gitlab-ci tags for this runner (comma separated):`给这个`Runner`指定`tags`，稍后也可以在`GitLab's UI`中修改。

`Whether to run untagged builds [true/false]:`选择`Runner`是否接受未指定`tags`的任务,稍后可修改。默认值为`false`。

`Whether to lock the Runner to current project [true/false]:` 选择是否为当前项目锁定Runner，可修改。通常用于被指定为某个项目的Runner，默认值为`true`。

`Please enter the executor: docker, shell, virtualbox, kubernetes, docker-ssh, parallels, ssh, docker+machine, docker-ssh+machine:` 选择Runner executor(Runner执行器)，使用`shell`,使用默认本地环境。

## pep8检查环境配置

在runner运行的服务器上配置python环境，基本上各大Linux发行版都默认带有python2版本。

CentOS安装pip的方法，使用命令`sudo yum install python-pip`安装，如果没有`python-pip`包，name先安装`epel`仓库`sudo yum install epel-release`,接受指纹。之后再执行一次命令安装。

使用pip安装`flake8`和`pep8`，推荐使用`pipenv`安装。直接安装的方式`sudo pip install pep8 flake8`，第一次使用`pip`可能需要更新`sudo pip install --upgrade pip`，如果不使用`sudo`会报`权限不足`。
更改`pip`源，提高下载速度。编辑`$HOME/.pip/pip.conf`，添加内容：
`[global]
index-url = https://mirrors.ustc.edu.cn/pypi/web/simple
format = columns`
如果文件不存在，创建新文件或目录。


### 在项目中使用flake8进行风格检查

需要在项目根目录下添加两个文件`.flake8`和`.gitlab-ci.yml`，提交到`gitlab`上。

### 添加`.flake8`配置文件

```
[flake8]
ignore = W292
exclude =
    *migrations*,
    # python related
    *.pyc,
    .git,
    __pycache__,
max-line-length=120
max-complexity=12
format=pylint
show_source = True
statistics = True
count = True
```

说明
```
注意, .flake8里面不要带中文
ignore = 忽略错误类型
exclude = 不检查的文件正则列表
max-line-length = 单行最大字符数120
max-complexity = 代码复杂度等级
format = 展示格式
show_source = 显示源代码
statistics = 展示统计
count = 展示总错误数
```

本地运行检测测试`$ flake8 .`。

### 添加`.gitlab-ci.yml`配置文件

```
before_script:
  - echo "Python静态代码检查..."

pep8:
  script:
    - flake8 .
```
