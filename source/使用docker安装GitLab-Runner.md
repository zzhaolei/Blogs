---
title: 使用docker安装GitLab-Runner
copyright: true
date: 2018-05-06 10:06:17
tags:
  - docker
  - GitLab-Runner
categories:
---

# docker安装gitlab-runner

`docker pull gitlab/gitlab-runner:latest`安装`gitlab-runner`

打开自己搭建的`GitLab`网站，点击顶栏的`Snippets`后面的小扳手，再点击左侧列表中`Overview`中的`Runners`，在打开的网页下面，可以看到`How to setup a shared Runner for a new project`行，`2是Runners设置时需要指定的URL,3是在设置是的Runners`。

## 运行镜像

`docker run -d --name gitlab-runner --restart always -v /var/run/docker.sock:/var/run/docker.sock -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner:latest`

## 注册`gitlab-runner`
`docker exec -it gitlab-runner gitlab-runner register`，

`Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):`输入域名或者服务器`ip`地址，格式为`https://gitlab.com`。和`token` `Please enter the gitlab-ci token for this runner:`。

`Please enter the gitlab-ci description for this runner:`输入`runner`描述。

`Please enter the gitlab-ci tags for this runner (comma separated):`给这个`Runner`指定`tags`，稍后也可以在`GitLab's UI`中修改。

`Whether to run untagged builds [true/false]:`选择`Runner`是否接受未指定`tags`的任务,稍后可修改。默认值为`false`。

`Whether to lock the Runner to current project [true/false]:` 选择是否为当前项目锁定Runner，可修改。通常用于被指定为某个项目的Runner，默认值为`true`。

`Please enter the executor: docker, shell, virtualbox, kubernetes, docker-ssh, parallels, ssh, docker+machine, docker-ssh+machine:` 选择Runner executor(Runner执行器)，使用`shell`,使用`gitlab-runner`环境。
重启容器`docker restart gitlab-runner`。

修改配置文件`docker exec -it name vim /etc/gitlab-runner/config.toml`

## pep8检查环境配置

进去docker容器环境`docker exec -it name /bin/bash`，`name`就是容器的名称，如果没有启动容器，会报错。

安装`python-pip`，`apt-get update && apt-get install python-pip -y`。

使用pip安装`flake8`和`pep8`，`pip install pep8 flake8`，第一次使用`pip`可能需要更新`pip install --upgrade pip`。
更改`pip`源，提高下载速度。编辑`$HOME/.pip/pip.conf`，添加内容：
`[global]
index-url = https://mirrors.ustc.edu.cn/pypi/web/simple
format = columns`
如果文件不存在，创建新文件或目录。


### 在项目中使用flake8进行风格检查

需要在项目根目录下添加两个文件`.flake8`和`.gitlab-ci.yml`，提交到`gitlab`上。

### 添加`.flake8`配置文件

`[flake8]
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
count = True`

说明
```text
注意, 在.flake8里面不要带中文
ignore = 忽略错误类型
exclude = 不检查的文件正则列表
max-line-length = 单行最大字符数120
max-complexity = 代码复杂度等级
format = 展示格式
show_source = 显示源代码
statistics = 展示统计
count = 展示总错误数
```

### 添加`.gitlab-ci.yml`配置文件

```yaml
before_script:
  - echo "Python静态代码检查..."
pep8:
  script:
    - flake8 .
```
