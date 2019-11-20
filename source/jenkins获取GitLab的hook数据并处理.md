---
title: jenkins获取GitLab的hook数据并处理
copyright: true
date: 2018-08-24 14:55:22
tags:
  - Jenkins
  - GitLab
  - webhook
categories:
---

# jenkins获取GitLab的hook数据并处理

`jenkins`安装`Generic Webhook Trigger Plugin`插件

{% asset_img Generic_Webhook_Trgger.png Generic\ Webhook\ Trgger具体配置 %}

创建任务，在`构建触发器中`勾选`Generic Webhook Trigger`, 在展开的页面中会有一个`URL`, 这个`URL`是配置`Webhook`使用的，一般形式是`http://JENKINS_URL/generic-webhook-trigger/invoke`, `JENKINS_URL`是初始配置`jenkins`时会有的，一般是`http://IP:8080`的形式。

如果之前有配置过`GitLab webhook`, 请去除这个，不然`Generic Webhook Trigger`不会生效。

如果`Git`仓库的验证使用的是用户名和密码，那么就不需要配置`Token`.

复制这个`URL`，将`JENKINS_URL`替换为自己的真实数据，在`GitLab`的对应的项目的`设置-->集成`中输入，没有安全令牌，勾选自己需要的触发器事件，SSL证书验证可以开启，也可以不开启，推荐开启。

任务设置界面还有`Post content parameters`, `Header parameters`, `Request parameters`, 分别是请求的`post数据`，`header头`，`请求头`。

一般我们只需要`post`的数据里的内容就够了。

图片的3，4分别是`变量名（3）`，`值（4）`。值是直接从`post`的`json格式`的数据中取出来的，注意，值的获取方式是`JSONPath`，并且里面有`点`，如果`json`数据有多层，那么就多点几次，比如`$.project.name`等，可以配置多个变量组。

我们还可以`Generic Webhook Trigger`配置`Optional filter`，设置一个触发事件，比如只有当你推送的分支是`develop`的时候，才去构建。

在`Optional filter`的`Expression`框中输入`^(refs/heads/develop)$`, 在下面的`Text`框中输入`步骤3的变量名`，这里写`$ref`。

在`shell`中使用变量名的方式也是直接使用，例如`echo $ref`， 注意没有`点`。

配置完成后，保存，可以点击刚刚配置的`GitLab`的`webhook`页面的`Test`对应的事件测试。

`GitLab`的`push`的`hook`数据：
```
{
  "object_kind": "push",
  "before": "95790bf891e76fee5e1747ab589903a6a1f80f22",
  "after": "da1560886d4f094c3e6c9ef40349f7d38b5d27d7",
  "ref": "refs/heads/master",
  "checkout_sha": "da1560886d4f094c3e6c9ef40349f7d38b5d27d7",
  "user_id": 4,
  "user_name": "John Smith",
  "user_username": "jsmith",
  "user_email": "john@example.com",
  "user_avatar": "https://s.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=8://s.gravatar.com/avatar/d4c74594d841139328695756648b6bd6?s=80",
  "project_id": 15,
  "project":{
    "id": 15,
    "name":"Diaspora",
    "description":"",
    "web_url":"http://example.com/mike/diaspora",
    "avatar_url":null,
    "git_ssh_url":"git@example.com:mike/diaspora.git",
    "git_http_url":"http://example.com/mike/diaspora.git",
    "namespace":"Mike",
    "visibility_level":0,
    "path_with_namespace":"mike/diaspora",
    "default_branch":"master",
    "homepage":"http://example.com/mike/diaspora",
    "url":"git@example.com:mike/diaspora.git",
    "ssh_url":"git@example.com:mike/diaspora.git",
    "http_url":"http://example.com/mike/diaspora.git"
  },
  "repository":{
    "name": "Diaspora",
    "url": "git@example.com:mike/diaspora.git",
    "description": "",
    "homepage": "http://example.com/mike/diaspora",
    "git_http_url":"http://example.com/mike/diaspora.git",
    "git_ssh_url":"git@example.com:mike/diaspora.git",
    "visibility_level":0
  },
  "commits": [
    {
      "id": "b6568db1bc1dcd7f8b4d5a946b0b91f9dacd7327",
      "message": "Update Catalan translation to e38cb41.",
      "timestamp": "2011-12-12T14:27:31+02:00",
      "url": "http://example.com/mike/diaspora/commit/b6568db1bc1dcd7f8b4d5a946b0b91f9dacd7327",
      "author": {
        "name": "Jordi Mallach",
        "email": "jordi@softcatala.org"
      },
      "added": ["CHANGELOG"],
      "modified": ["app/controller/application.rb"],
      "removed": []
    },
    {
      "id": "da1560886d4f094c3e6c9ef40349f7d38b5d27d7",
      "message": "fixed readme",
      "timestamp": "2012-01-03T23:36:29+02:00",
      "url": "http://example.com/mike/diaspora/commit/da1560886d4f094c3e6c9ef40349f7d38b5d27d7",
      "author": {
        "name": "GitLab dev user",
        "email": "gitlabdev@dv6700.(none)"
      },
      "added": ["CHANGELOG"],
      "modified": ["app/controller/application.rb"],
      "removed": []
    }
  ],
  "total_commits_count": 4
}

```
