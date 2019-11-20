---
title: vim配置轻量级IDE
copyright: true
date: 2018-05-06 09:39:38
tags:
  - VIM
  - Vundle
categories:
---

# 安装VIM

安装`YouCompleteMe`的时候, 需要`VIM`的版本比较高, 一般一些`Linux`长期支持版的`Vim`包就比较老, 所以可能需要编译安装.

卸载之前系统中安装的版本:
```
CentOS/Fedora
    sudo yum remove vim -y

Debian/Ubuntu
    sudo apt autoremove vim -y
```

[Vim ftp](ftp://ftp.vim.org/pub/vim/unix), 查找需要的版本, 一般选择最新版本就行了.

下载, 解压后, 进入目录中的`src`目录. 如果使用默认的配置的话, 那么直接运行:
```
make
sudo make install
```

想自定义的话, 需要使用`./configure`重新编译.
`./configure --with-features=huge --enable-pythoninterp`

 - `--with-features=huge`: 最大的特性支持.
 - `--enable-pythoninterp`: 启用`Vim`对`Python`的支持, 默认`Vim`是不支持`Python`的.

然后重新执行:
```
make
sudo make install
```

卸载安装的`Vim`, 在`src`目录中执行`sudo make uninstall`. 如果想要重新编译的话, 那么需要清除之前编译的文件`make distclean`.

# 将VIM配置为轻量级IDE

`YouCompleteMe`需要系统安装`ctags`, `gcc`, `gcc-c++`, `python-devel`, `cmake`。

`YouCompleteMe`如果报错`The ycmd server SHUT DOWN...`, 那么进入`~/.vim/bundle/YouCompleteMe`目录下, 执行`./install.py`

克隆`VundleVim`到`~/.vim/bundle`, 会自动创建目录.
`git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`

首先在`vim`的配置文件`~/.vimrc`中关闭兼容模式和文件类型检测, 直接在文件中追加:
```
set nocompatible
filetype off
```

 - `VundleVim` 管理插件
 - `YouCompleteMe` 代码自动补全
 - `vim-airline` 增强状态栏和标签栏
 - `Tagbar` 查看函数和变量的定义
 - `NERDTree` 文件目录树
 - `identline` 显示缩进的对齐线
 - `delimitMate` 自动补全括号和引号
 - `Cscope` 代码阅读

还支持两种插件类型:
```
" 非 github 仓库的插件
Plugin 'git://git.wincent.com/command-t.git'

" 本地仓库的插件
Plugin 'file:///home/lei/plugin'
```

`VundleVim`的简单命令, 在`vim`的命令模式中执行, 例如(`:PluginInstall`):
 - `PluginList`: 列出安装的插件
 - `PluginInstall`: 根据配置文件安装插件
 - `PluginUpdate`: 更新插件
 - `PluginSearch [name]`: 查找名为 [name] 的插件
 - `PluginClean`: 清除无用的插件

保存`.vimrc`, 在`命令模式`中输入`:PluginInstall`, 安装配置的需要安装的插件。

## VIM配置文件

如果不安装插件那么直接复制`基本`的配置内容就行了.

如果想全局生效, 那么在`/etc/vimrc`中进入插入模式, 在文件最后面追加配置。

配置vim
```

" =======================
" vimrc配置
" =======================

" -----------------------
" => 基本
" -----------------------

" 显示行号
set nu

" 高亮关键字
syntax on

" 使用鼠标
set mouse=a

" 搜索结果高亮显示
set hlsearch

" 一个tab等于4个空格
set tabstop=4
set shiftwidth=4

" 表示Tab自动转换成空格
set expandtab

" 智能对齐
set autoindent
set smartindent

" 解决删除键不生效
set backspace=indent,eol,start

" 行尾结束符, 设置为unix
set fileformat=unix

" 文件编码
" fenc=utf-8

" 配置粘贴不自动换行的快捷键
set pastetoggle=<F9>

" 允许折叠
set foldenable

" 手动折叠
set foldmethod=manual

" 自动保存
set autowrite

" 记录历史的行数
set history=10000

" 设置搜索时忽略大小写
set ignorecase

" 高亮显示光标所在的行和列
" 显示颜色
set t_Co=256
set bg=dark
set cursorline
set cursorcolumn

" 自定义光标样式
highlight CursorLine cterm=NONE ctermbg=black ctermfg=brown guibg=NONE guifg=NONE
highlight CursorColumn cterm=NONE ctermbg=black ctermfg=brown guibg=NONE guifg=NONE

" 显示tab和空格
set list

" 设置tab和空格样式
set lcs=tab:\|\ ,nbsp:%,trail:·

" 设定行首tab为灰色
highlight LeaderTab guifg=#666666

" 匹配行首tab
match LeaderTab /^\t/

" 输入的命令显示出来
set showcmd

" 启动显示状态行(1),总是显示状态行(2)
set laststatus=2

" -----------------------
" => 安装插件
" -----------------------

" 关闭vi兼容模式
set nocompatible

" 关闭文件类型检测
filetype off

" 运行时路径, 插件
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" 插件管理器
Plugin 'VundleVim/Vundle.vim'

" -----------------------
" => github 仓库中的插件
" -----------------------
" 自动补全
Plugin 'Valloric/YouCompleteMe'
" 底栏和顶栏
Plugin 'vim-airline/vim-airline'
" 侧边栏显示git修改
Plugin 'airblade/vim-gitgutter'
" 目录树显示文件的git修改
Plugin 'Xuyuanp/nerdtree-git-plugin'

" -----------------------
" => vim-scripts 中的插件
" -----------------------
" 显示文件中所有的函数和变量
Plugin 'Tagbar'
" 目录树
Plugin 'The-NERD-tree'
" 显示tab对齐线
Plugin 'indentLine.vim'
" 符号自动补全
Plugin 'delimitMate.vim'
" 标签提取
Plugin 'ctags.vim'
" 颜色主题
Plugin 'desert.vim'
" 显示Python的虚拟环境
Plugin 'virtualenv.vim'

call vundle#end()
filetype plugin indent on


" -----------------------
" => 插件安装完成后的配置
" -----------------------

" -----------------------
" ==> airline 标签栏配置
" -----------------------

" 启用powerline字体
let g:airline_powerline_fonts = 1
" 开启标签栏
let g:airline#extensions#tabline#enabled = 1
" 标签栏路径显示
" let g:airline#extensions#tabline#formatter = "default"
" let g:airline#extensions#tabline#formatter = "jsformatter"
" let g:airline#extensions#tabline#formatter = "unique_tail_improved"
let g:airline#extensions#tabline#formatter = "unique_tail"

" -----------------------
" ==> YouCompleteMe配置
" -----------------------
" YouCompleteMe 默认tab, s-tab和自动补全冲突
let g:ycm_key_list_select_completion=['<c-n>']
let g:ycm_key_list_select_completion = ['<Down>']
let g:ycm_key_list_previous_completion=['<c-p>']
let g:ycm_key_list_previous_completion = ['<Up>']

set completeopt=longest,menu

" 关闭加载.ycm_extra_conf.py提示
let g:ycm_confirm_extra_conf=0

" 在注释输入中也能补全
let g:ycm_complete_in_comments = 1
" 在字符串输入中也能补全
let g:ycm_complete_in_strings = 1
" 直接触发自动补全
let g:ycm_key_invoke_completion = '<C-Space>'

" 显示所有匹配
map <leader>gd :YcmDiags<CR>
" 跳转到申明处
nnoremap <leader>gl :YcmCompleter GoToDeclaration<CR>
" 跳转到定义处
nnoremap <leader>gf :YcmCompleter GoToDefinition<CR>
" 函数定义跳转, 将光标定位到函数, 输入gd 回车
nnoremap <leader>gg :YcmCompleter GoToDefinitionElseDeclaration<CR>

" -----------------------
" ==> Tagbar配置
" -----------------------
" 函数变量窗口, F3开启和关闭
map <F3> :TagbarToggle<CR>
let tagbar_ctags_bin = 'ctags'
let tagbar_right = 1
let g:tagbar_width = 30
" tagbar一打开, 光标就在tagbar页面内
let g:tagbar_autofocus = 1
" 设置标签不排序，默认排序
let g:tagbar_sort = 0

" -----------------------
" ==> The-NERD-tree配置
" -----------------------
" 目录树, 使用F2开启或关闭
map <F2> :NERDTreeToggle<CR>
let NERDTreeWinSize=30
" 自动开启
" autocmd vimenter * NERDTree
" 是否显示隐藏文件
let NERDTreeShowHidden=1
" 忽略一下文件的显示
let NERDTreeIgnore=['\.pyc','\~$','\.swp']

" -----------------------
" ==> Xuyuanp/nerdtree-git-plugin配置
" -----------------------
let g:NERDTreeIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ 'Ignored'   : '☒',
    \ "Unknown"   : "?"
    \ }

" -----------------------
" ==> vim-gitgutter配置
" -----------------------


" -----------------------
" ==> virtualenv.vim
" -----------------------

" 退出虚拟环境
" :VirtualEnvDeactivate
" 显示所有虚拟环境
" :VirtualEnvList
" 激活虚拟环境
" :VirtualEnvActivate venv
" 可以使用tab选择要激活的虚拟环境
" :VirtualEnvActivate <tab>
" 显示帮助
" :help virtualenv

```

`highlght`主要是用来配色的, 包括语法高亮等个性化的配置. 可以通过`:h highlight`, 查看详细信息.

`CursorLine`和`CursorColumn`分别表示当前所在的行列.

`cterm`表示为原生`vim`设置样式, 设置为NONE表示可以自定义设置.

`ctermbg`设置终端`vim`的背景色.

`ctermfg`设置终端`vim`的前景色.

`guibg`和`guifg`分别是设置`gvim`的背景色和前景色, 本人平时都是使用终端打开vim, 所以只是设置终端下的样式.

推荐使用的颜色包括: `black`, `brown`, `grey`, `blue`, `green`, `cyan`, `magenta`, `yellow`, `white`.
