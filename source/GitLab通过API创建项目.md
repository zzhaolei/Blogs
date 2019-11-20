---
title: GitLab通过API创建项目
copyright: true
date: 2018-08-31 15:15:31
tags:
categories:
---

# 


示例:
```
#!/usr/bin/python
# -*- coding:utf-8 -*-


import os
import json
import requests
import subprocess

from ast import literal_eval


root = "/root/develop"

headers = {
    "PRIVATE-TOKEN": "aljdhaguiqwackjaerigczx"
}

fromdata = {
    "namespace_id": "41"
}

url = "http://ip:port/api/v4/projects"

def delete():
    """删除项目"""
    response = requests.get(url, headers=headers)
    tmp = json.loads(response.text)
    for i in tmp:
        project_name = i["name"]
        requests.delete("http://ip:port/api/v4/projects/myscan-master%2f{}".format(project_name), headers=headers)

def create():
    """创建项目，复制代码"""
    os.chdir("./deploy/")
    names = os.listdir("./")
    for i in names:
        if os.path.isdir(i):
            if i == ".git":
                continue

            fromdata["name"] = i
            fromdata["path"] = i

            if i != "component_all":
                os.chdir(i)
                subprocess.call("rm -rf .git build dist *.egg-info .idea .DS_Store ", shell=True)
                pwd_1 = os.getcwd()

                ssh_url = requests.post(url, data=fromdata, headers=headers)
                subprocess.call("git clone -b develop {} {}/{}".format(ssh_url, root, i), shell=True)
                os.chdir("{}/{}".format(root, i))
                pwd_2 = os.getcwd()
                subprocess.call("cp -rf {}/* {}/".format(pwd_1, pwd_2), shell=True)
                subprocess.call("git push --set-upstream origin develop", shell=True)
                subprocess.call('git add -A && git commit -m "create" && git push'.format(j), shell=True)

                subprocess.call("rm -rf {}/{}".format(root, i))
                os.chdir(pwd_1)
                os.chdir("../")
            else:
                pass

def component():
    """组件"""
    fromdata["name"] = "component_all"
    fromdata["path"] = "component_all"
    ssh_url = requests.post(url, data=fromdata, headers=headers)
    subprocess.call("git clone -b develop {} {}/component_all".format(ssh_url, root), shell=True)

    os.chdir("./deploy/component_all")
    path = os.getcwd()
    names = os.listdir("./")
    for i in names:
        os.chdir("{}/{}".format(path, i))
        if os.path.isdir(i):
            if i == ".git" or i == "README.md":
                continue

            subprocess.call("rm -rf .git build dist *.egg-info .idea .DS_Store ", shell=True)
            pwd_1 = os.getcwd()

            os.makedirs("{}/component_all/{}".format(root, i))
            os.chdir("{}/component_all/{}".format(root, i))
            pwd_2 = os.getcwd()
            subprocess.call("cp -rf {}/* {}/".format(pwd_1, pwd_2), shell=True)
            subprocess.call("git push --set-upstream origin develop", shell=True)
            subprocess.call('git add -A && git commit -m "添加{}组件" && git push'.format(i), shell=True)


if __name__ == '__main__':
    # 删除项目
    delete()

    # 复制项目
    create()

    # 复制组件
    component()


```
