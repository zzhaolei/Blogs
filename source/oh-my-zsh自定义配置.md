---
title: oh-my-zsh自定义配置
copyright: true
date: 2018-06-22 15:45:32
tags:
  - zsh
  - oh-my-zsh
  - Theme
categories:
---

# oh-my-zsh主题配置

默认的`zsh`主题`robbyrussell`已经很棒了, 简洁高效, 能很好的显示`git`的相关信息, 比如`branch`信息, `修改`, `删除`, `添加`等操作. 但是多用户的话就不能很好的展示, 我们可以通过修改`robbyrussell`的配置文件来达到我们想要的效果.

一般情况下, 我们安装`oh-my-zsh`都是在自己的家目录下`~`, 所以我们就以家目录为例.

编辑`~/.oh-my-zsh/themes/robbyrussell.zsh-theme`这个文件:
```
local ret_status="%(?:%{$fg_bold[green]%}➜ :%{$fg_bold[red]%}➜ )"
PROMPT='${ret_status} %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"
```

将`local ret_status="%(?:%{$fg_bold[green]%}➜ :%{$fg_bold[red]%}➜ )"`修改为`local ret_status="%(?:%{$fg_bold[yellow]%}${USER} %{$fg_bold[green]%}➜ :%{$fg_bold[yellow]%}${USER} %{$fg_bold[red]%}➜ )"`

如下:
```
local ret_status="%(?:%{$fg_bold[yellow]%}${USER} %{$fg_bold[green]%}➜ :%{$fg_bold[yellow]%}${USER} %{$fg_bold[red]%}➜ )"
PROMPT='${ret_status} %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"
```

这样当使用`su`切换时用户名还是原来的, 添加一个判断:
```
if [ `id -u` -eq 0 ];then
    local ret_status="%(?:%{$fg_bold[yellow]%}root %{$fg_bold[green]%}➜ :%{$fg_bold[yellow]%}root %{$fg_bold[red]%}➜ )"
else
    local ret_status="%(?:%{$fg_bold[yellow]%}${USER} %{$fg_bold[green]%}➜ :%{$fg_bold[yellow]%}${USER} %{$fg_bold[red]%}➜ )"
fi
PROMPT='${ret_status} %{$fg[cyan]%}%c%{$reset_color%} $(git_prompt_info)'

ZSH_THEME_GIT_PROMPT_PREFIX="%{$fg_bold[blue]%}git:(%{$fg[red]%}"
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%} "
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[blue]%}) %{$fg[yellow]%}✗"
ZSH_THEME_GIT_PROMPT_CLEAN="%{$fg[blue]%})"
```

保存退出, 在终端中执行`source .zshrc`让配置生效.

样式为"`root ➜  ~ `".
