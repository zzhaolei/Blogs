---
title: chromedriver配置
copyright: true
date: 2018-05-06 09:44:02
tags:
  - chromedriver
categories:
---

# chromedriver配置

需要先安装chrome浏览器，添加chrome源

`sudo vim /etc/yum.repos.d/google-chrome.repo`

添加以下内容
```
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=0
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```

安装`yum -y install google-chrome-stable`，国内可能无法访问，使用`yum -y install google-chrome-stable --nogpgcheck`或者把`gpgcheck`改为`0`。

下载`chromedriver`，解压放在`/usr/bin/`下。

`which chromedriver`测试是否成功。

如果报`selenium.common.exceptions.WebDriverException: Message: unknown error: DevToolsActivePort file doesn't exist`, 或者类似的错误, 尝试在初始化时添加参数`--no-sandbox`和`--disable-dev-shm-usage`, 例如:
```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options


options = Options()
options.add_argument("--headless")
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')
options.add_argument(
    'user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"')
driver = webdriver.Chrome(chrome_options=options)
driver.get("https://www.baidu.com")
```
