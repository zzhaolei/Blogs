---
title: Visual Studio Code配置
copyright: true
date: 2018-05-06 10:00:11
tags:
  - Visual Studio Code
categories:
---

# Visual Studio Code

从`1.23.0`开始`VS Code`就不再默认提供各语言版本, 而是改为使用插件的方式提供语言包.

在插件商店搜索`Chinese (Simplified)`, 安装.

## 同步Visual Studio Code插件和配置

 - 使用`ctrl+shift+X`打开扩展搜索, 搜索安装`Settings Sync`, 安装完成后点击`重新加载`.

 - 在`VS code`中使用快捷键`alt+shift+u`, 会自动跳转到浏览器, 访问[这个](https://github.com/settings/tokens)链接. 点击`Generate new token`, 然后输入`Token description`, 就是名字, `Select scopes`勾选`gist`, 然后点击`Generate token`, 会生成一个`token`, 复制. 把复制的`token`粘贴到`VS code`中回车, 会上传配置和插件信息, 上传完成后会出现类似这样的信息:
    ```
    CODE SETTINGS SYNC UPLOAD SUMMARY
    Version: 2.9.2
    --------------------
    GitHub Token: xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    GitHub Gist: xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    GitHub Gist Type: Secret

    Restarting Visual Studio Code may be required to apply color and file icon theme.
    --------------------
    Files Uploaded:

      Extensions Removed:

    Extensions Added:
      code-settings-sync v2.9.2
      vscode-icons v7.23.0
      vscode-language-pack-zh-hans v1.23.4
    --------------------
    Done.
    ```

 - 在[GitHubGist](https://gist.github.com/)上, 会有一个`cloudSettings`, 打开[链接](https://github.com/settings/tokens), 点击刚创建的`token`, 在描述信息`Token description`后添加`xxxx_<GitHubGist>`, 这样可以防止遗忘了`GitHubGist`的值.

 - 复制`GitHub Gist`的信息, 在新的电脑上安装`Settings Sync`, 打开配置`alt+shift+u`, 将`GitHub Gist`的值粘贴进去, 等待下载同步.


## Visual Studio Code配置

 - Visual Studio Code快捷键

     - 写`markdown`边写边预览的快捷键: `ctrl + k`, 再按`v`.

 -  Visual Studio Code插件

     - `vscode-icons` or `VSCode Great Icons`
     - `Python`
     - `Code Runner`
     - `Chinese (Simplified)`

