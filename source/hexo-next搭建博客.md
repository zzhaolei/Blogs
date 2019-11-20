---
title: hexo + next搭建博客
copyright: true
date: 2018-05-06 09:36:45
tags:
  - hexo
  - next
  - npm
  - GitHub
  - js
categories:
---

# 使用hexo和next主题搭建GitHub博客

## 安装node.js

在`node.js` [官网](https://nodejs.org/en/)下载, 相应系统位数的软件包. 可以选择`LTS`长期支持版, 或者`Current`当前版本. 我这里选择的是`Windows 64`的`Current`版本. 

双击运行安装程序, 会提示`please wait while the setup wizard prepares to guide you through the installation.`, 等待一会. 

之后一路点击`next`就行了. 注意:`记得勾选Add to PATH`, 一般使用默认配置就行了. 授予管理员权限, 等待安装完毕.

## 测试是否安装成功

使用`Win + R`键, 在运行弹出框中, 输入`cmd`回车, 就能进入`命令提示符`, 输入`node -v`, 会输出版本号.

## 安装hexo

在`命令提示符`中输入:

```
# 安装hexo, 执行完成后, 可能会显示WARN, 这个其实无关大局, 可以忽略.
npm install hexo-cli -g

# blog是你存放博客的目录, hexo会初始化一些相关的数据
hexo init blog

# 进入目录
cd blog

# 安装一些相关的依赖
npm install

# 起一个hexo服务, 可以在本地预览效果
hexo server

# 会有类似的提示, 在浏览器中输入http://localhost:4000/, 就能访问
# 动态刷新, 当修改了文件时, 可以即时的响应.
# 使用Ctrl + C 结束服务
INFO  Start processing  
INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.

# 清除缓存
hexo clean
```

# 配置hexo多机器更新博客

## 安装Git工具

`Windows`在 [官网](https://git-scm.com/download/win) 下载对应系统位数的安装包, 之后点击安装.

`Debian/Ubuntu`: `sudo apt install git -y`.

`Fedora/CentOS`: `sudo yum install git -y`

## 配置Github

注册`Github`的账号, 在登录后的首页上点击`New repository`, 创建新的仓库.

`Repository name`为`用户名.github.io`, 我这里是`zZhaolei.github.io`, 

`Description`是可选的描述信息. 选择`Public`, 勾选`Initialize this repository with a README`, 这样就有了第一次提交. 点击`Create repository`, 完成创建.

点击`clone or download`, 复制`HTTPS`链接, 在系统上使用`git clone https://github.com/xxxxxxxx.git`, 然后进入`clone`下来的目录, 使用`git branch`查看当前的所有分支.

创建一个`hexo`分支用来存放源码, `git branch hexo`.

使用`git checkout hexo`, 切换分支到`hexo`.

将`blog`目录下的所有文件复制到`hexo`分支下.

将`themes/landscape`删除, 当`git add .`的时候会报错. 或者把`themes/landscape/.git`目录删除.

~~注意修改`.gitignore`文件, 将其中的`node_modules`、`public`、~~

## 新建一篇博客
`hexo new post "hello world"`

## 生成部署
`hexo d -g`


# 配置必要的信息

## 配置Deployment

注意: `缩进都是两个空格, 冒号后面一定要有一个空格`

在`_config.yml`文件中, 找到`Deployment`项, 按照如下格式修改:
```
deploy:
  type: git
  repo: https://github.com/zZhaoLei/zZhaolei.github.io.git
  branch: master
```
`branch`是`hexo`部署的分支.

## 配置title
在`_config.yml`文件中, 找到`Site`, 更改`title`的值为`xx的博客`, 随意写, 这个是浏览器的标签上显示的内容, 更改`author`作者. 设置`language`为`zh-Hans`

## 使用next主题
~~在项目根目录下, 使用~~

~~ `git submodule add https://github.com/iissnan/hexo-theme-next themes/next` ~~

~~添加`git 子模块`.~~

`git clone https://github.com/iissnan/hexo-theme-next themes/next`

删除`themes/next/`下面的`.git`、`.github`、`.gitignore`、`.gitattribute`

修改项目根目录的`_config.yml`, 将`themes`的值改为`next`.

## 设置主题信息

 - 切换布局:  
    修改`themes/next/_config.yml`中的`scheme`字段的值, 有四种字段可以选择, 根据自己的喜欢选择, 我选择的是`scheme: Muse`.

 - 设置菜单:  
    打开`themes/next/_config.yml`文件，找到`menu`字段. 根据自己的需要修改响应的值.

 - 菜单项显示中文:  
    修改`themes/next/languages/`下的对应的`.yml`文件, 我之前将语言设置为了`zh-Hans`, 所以这里选择`zh-Hans.yml`. 修改`menu`下的属性值.

 - 设置侧栏:  
    修改`themes/next/_config.yml`中的`sidebar`字段的`position`值, 有`left`和`right`.

 - 设置头像:  
    在`themes/next/_config.yml`中添加新的字段`avatar`,  值设置成头像的链接地址. 其中, 头像的链接地址可以是: `http://example.com/avtar.png`, 本地文件: 放置在`source/images/`目录下配置为: `avatar: /images/avatar.png`.

# 个性化配置

参考`next`官方[配置文档](http://theme-next.iissnan.com/getting-started.html)

## 在右上角或者左上角实现`fork me on github`

在[github-ribbons](https://blog.github.com/2008-12-19-github-ribbons/)或者[github-corners](http://tholman.com/github-corners/), 找到自己
喜欢的样式, 复制右侧选框中的所有内容, 如下:
```
<a href="https://your-url" class="github-corner" aria-label="View source on Github"><svg width="80" height="80" viewBox="0 0 250 250" style="fill:#151513; color:#fff; position: absolute; top: 0; border: 0; right: 0;" aria-hidden="true"><path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"></path><path d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2" fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"></path><path d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z" fill="currentColor" class="octo-body"></path></svg></a><style>.github-corner:hover .octo-arm{animation:octocat-wave 560ms ease-in-out}@keyframes octocat-wave{0%,100%{transform:rotate(0)}20%,60%{transform:rotate(-25deg)}40%,80%{transform:rotate(10deg)}}@media (max-width:500px){.github-corner:hover .octo-arm{animation:none}.github-corner .octo-arm{animation:octocat-wave 560ms ease-in-out}}</style>
```

然后粘贴刚才复制的代码到`themes/next/layout/_layout.swig`文件中(放在`<div class="headband"></div>`的下面), 并把`href`改为你的`github`地址(`个人主页`)

## 小屏幕不显示fork me on github

修改文件`hexo`博客根目录`themes\next\layout\_layout.swig`找到如下代码块:
```
<html class="{{ html_class | lower }}">
<head>
  {% include '_partials/head.swig' %}
  <title>{% block title %}{% endblock %}</title>
```

添加如下代码:
```
  <style>
    .forkme{
   display: none;
 }
  @media (min-width: 768px) {
   .forkme{
    display: inline;
   }
  }
  </style>
```

在之前添加的`fork me on github`代码块上套上`div`加上`class`就行了，如下
```
<div class="forkme">
  <a href="https://https://github.com/zZhaoLei" class="github-corner" aria-label="View source on Github"><svg width="80" height="80" viewBox="0 0 250 250" style="fill:#151513; color:#fff; position: absolute; top: 0; border: 0; right: 0;" aria-hidden="true"><path d="M0,0 L115,115 L130,115 L142,142 L250,250 L250,0 Z"></path><path d="M128.3,109.0 C113.8,99.7 119.0,89.6 119.0,89.6 C122.0,82.7 120.5,78.6 120.5,78.6 C119.2,72.0 123.4,76.3 123.4,76.3 C127.3,80.9 125.5,87.3 125.5,87.3 C122.9,97.6 130.6,101.9 134.4,103.2" fill="currentColor" style="transform-origin: 130px 106px;" class="octo-arm"></path><path d="M115.0,115.0 C114.9,115.1 118.7,116.5 119.8,115.4 L133.7,101.6 C136.9,99.2 139.9,98.4 142.2,98.6 C133.8,88.0 127.5,74.4 143.8,58.0 C148.5,53.4 154.0,51.2 159.7,51.0 C160.3,49.4 163.2,43.6 171.4,40.1 C171.4,40.1 176.1,42.5 178.8,56.2 C183.1,58.6 187.2,61.8 190.9,65.4 C194.5,69.0 197.7,73.2 200.1,77.6 C213.8,80.2 216.3,84.9 216.3,84.9 C212.7,93.1 206.9,96.0 205.4,96.6 C205.1,102.4 203.0,107.8 198.3,112.5 C181.9,128.9 168.3,122.5 157.7,114.1 C157.9,116.9 156.7,120.9 152.7,124.9 L141.0,136.5 C139.8,137.7 141.6,141.9 141.8,141.8 Z" fill="currentColor" class="octo-body"></path></svg></a><style>.github-corner:hover .octo-arm{animation:octocat-wave 560ms ease-in-out}@keyframes octocat-wave{0%,100%{transform:rotate(0)}20%,60%{transform:rotate(-25deg)}40%,80%{transform:rotate(10deg)}}@media (max-width:500px){.github-corner:hover .octo-arm{animation:none}.github-corner .octo-arm{animation:octocat-wave 560ms ease-in-out}}</style>
</div>
```

## 添加`RSS`

然后安装`Hexo`插件：(这个插件会放在`node_modules`这个文件夹里)
在项目根目录下, 安装插件, 这个插件会放在`node_modules`这个文件夹里, 在`package.json`中会有备份.

```
npm install --save hexo-generator-feed
```

然后打开项目的配置文件`_config.yml`, 在`Extensions`字段后追加如下内容(`注意冒号后面要有空格`):
```
plugins: hexo-generate-feed
```
然后打开`themes/next/_config.yml`, 找到`rss`字段, 在`rss:` 后面添加`/atom.xml`, 注意冒号后面有个空格.

## 添加动态背景(粒子效果)

修改`themes/next/layout/_layout.swig`, 在`</body>`之前添加代码(注意不要放在`</head>`的后面), 一般在倒数第二行.
添加代码如下:
```
{% if theme.canvas_nest %}
  <script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

修改`themes/next/_config.yml`中的`canvas_nest`字段, 将值改为`true`.

如果觉得默认的线条太多的话, 可以将上一步代码代为:
```
{% if theme.canvas_nest %}
  <script type="text/javascript" color="0,0,255" opacity='0.7' zIndex="-2" count="99" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```
 - 配置项说明
 ```
    color：线条颜色, 默认: '0,0,0'; 三个数字分别为(R,G,B)
    opacity: 线条透明度(0~1), 默认: 0.5
    count: 线条的总数量, 默认: 150
    zIndex: 背景的z-index属性, css属性用于控制所在层的位置, 默认: -1
 ```

## 实现点击出现桃心效果

点击跳转并复制这个[js](http://7u2ss1.com1.z0.glb.clouddn.com/love.js)的内容, 新建`lova.js`, 并将内容粘贴进去.

保存在`themes/next/source/js/src`中, 然后打开`themes\next\layout\_layout.swig`, 在`</body>`之前添加以下代码`<script type="text/javascript" src="/js/src/love.js"></script>`. 

## 修改文章底部的那个带#号的标签

修改模板`themes/next/layout/_macro/post.swig`，搜索`rel="tag">#`, 将`#`换成`<i class="fa fa-tag"></i>`

## 博文压缩

在项目的根目录下执行
```
npm install gulp -g
npm install gulp-minify-css gulp-uglify gulp-htmlmin gulp-htmlclean gulp --save
```

新建`gulpfile.js`, 保存在根目录下, 添加以下内容:
```
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
// 压缩 public 目录 css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss())
        .pipe(gulp.dest('./public'));
});
// 压缩 public 目录 html
gulp.task('minify-html', function() {
  return gulp.src('./public/**/*.html')
    .pipe(htmlclean())
    .pipe(htmlmin({
         removeComments: true,
         minifyJS: true,
         minifyCSS: true,
         minifyURLs: true,
    }))
    .pipe(gulp.dest('./public'))
});
// 压缩 public/js 目录 js
gulp.task('minify-js', function() {
    return gulp.src('./public/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 执行 gulp 命令时执行的任务
gulp.task('default', [
    'minify-html','minify-css','minify-js'
]);
```

在生成博客时执行`hexo g && gulp`就会根据`gulpfile.js`中的配置, 对`public`目录中的静态资源文件进行压缩.

## 修改代码块自定义样式

打开`themes\next\source\css\_custom\custom.styl`, 添加(颜色可以自定义):
```
// Custom styles.
code {
    color: #ff7600;
    background: #fbf7f8;
    margin: 2px;
}
// 大代码块的自定义样式
.highlight, pre {
    margin: 5px 0;
    padding: 5px;
    border-radius: 3px;
}
.highlight, code, pre {
    border: 1px solid #d6d6d6;
}
```

## 侧边栏社交小图标设置

打开`themes/next/_config.yml`, 搜索`social:`, 在其中添加自己的链接, 如下.
```
social:
  GitHub: https://github.com/yourname || github
  E-Mail: mailto:yourname@gmail.com || envelope
```

~~搜索`social_icons:`, 在图标库找自己喜欢的小图标, 并将名字复制在如下位置, 保存即可.~~
```
social_icons:
  enable: true
  icons_only: false
  transition: false
  GitHub: spinner
```
`spinner`一个小绿点.

## 主页文章添加阴影效果

打开`themes\next\source\css\_custom\custom.styl`, 向其中追加内容如下:
```
// 主页文章添加阴影效果
.post {
  margin-top: 60px;
  margin-bottom: 60px;
  padding: 25px;
  -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
  -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
}
```

## 在网站底部加上访问量

打开`themes/next/layout/_partials/footer.swig`文件, 在`copyright`前(一般是第一行之前`<div class="copyright">`之前)加上:
```
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

然后在`<div class="powered-by">`里添加:
```
<div class="powered-by">
  <i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
  本站访客数: <span id="busuanzi_value_site_uv"></span>
  </span>
</div>
```

在这里有两中不同计算方式的统计代码:
 - `pv`的方式，单个用户连续点击`n`篇文章，记录`n`次访问量
```
<span id="busuanzi_container_site_pv">
  本站总访问量: <span id="busuanzi_value_site_pv"></span>次
</span>
```

 - `uv`的方式，单个用户连续点击`n`篇文章，只记录`1`次访客数
```
<span id="busuanzi_container_site_uv">
  本站访客数: <span id="busuanzi_value_site_uv"></span>
</span>
```

## 添加热度

`next`主题集成`leanCloud`, 打开`themes/next/layout/_macro/post.swig`, 搜索`leancloud-visitors-count`

在`leancloud-visitors-count`的`span`标签下添加新的`<span>℃</span>`.

再打开`themes/next/languages/zh-Hans.yml`, 将`visitors`的值改为`热度`.

## 网站底部字数统计

在项目根目录下, 执行:
```
npm install hexo-wordcount --save
```
然后在`themes/next/layout/_partials/footer.swig`的尾部追加:
```
<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">博客全站共 {{ totalcount(site) }} 字</span>
</div>
```

## 设置网站的图标Favicon

在[EasyIcon](www.easyicon.net)中找一张(`32*32`)的`ico`图标, 或者去别的网站下载或者制作, 并将图标名称改为`favicon.ico`, 然后把图标放在`themes/next/source/images`里, 并且修改主题配置文件:

```
# Put your favicon.ico into `hexo-site/source/` directory.
favicon: /favicon.ico
```

## 实现统计功能

在项目根目录下安装:
`npm install hexo-wordcount --save`

然后在`themes/next/_config.yml`中配置:
```
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
```

## 添加顶部加载条

打开`themes/next/layout/_partials/head.swig`文件, 在`<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1"/>`之后, 添加:
```
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
```

但是, 默认的是粉色的, 要改变颜色可以在`themes/next/layout/_partials/head.swig`文件中添加如下代码(接在刚才代码的后面):
```
<style>
    .pace .pace-progress {
        background: #1E92FB; /*进度条颜色*/
        height: 3px;
    }
    .pace .pace-progress-inner {
         box-shadow: 0 0 10px #1E92FB, 0 0 5px     #1E92FB; /*阴影颜色*/
    }
    .pace .pace-activity {
        border-top-color: #1E92FB;    /*上边框颜色*/
        border-left-color: #1E92FB;    /*左边框颜色*/
    }
</style>
```

也可以修改`themes/next/_config.yml`的`pace`的值为`true`, 修改`pace_theme`为你想要的样式.

## 在文章底部增加版权信息

在目录`themes/next/layout/_macro/`下添加`my-copyright.swig`, 内容如下:
```
{% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>
  
  <!-- JS库 sweetalert 可修改路径 -->
  <script src="https://cdn.bootcss.com/jquery/2.0.0/jquery.min.js"></script>
  <script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>
  <p><span>本文标题:</span><a href="{{ url_for(page.path) }}">{{ page.title }}</a></p>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script> 
    var clipboard = new Clipboard('.fa-clipboard');
      $(".fa-clipboard").click(function(){
      clipboard.on('success', function(){
        swal({   
          title: "",   
          text: '复制成功',
          icon: "success", 
          showConfirmButton: true
          });
        });
    });  
</script>
{% endif %}
```

在目录`themes/next/source/css/_common/components/post/`下添加`my-post-copyright.styl`, 内容如下:
```
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```

修改`themes/next/layout/_macro/post.swig`, 在代码:
```
{#####################}
{### END POST BODY ###}
{#####################}
```
之前添加如下代码:
```
<div>
  {% if not is_index %}
    {% include 'my-copyright.swig' %}
  {% endif %}
</div>
```

修改`themes/next/source/css/_common/components/post/post.styl`文件, 在最后一行增加代码:
```
@import "my-post-copyright;"
```

如果要在该博文下面增加版权信息的显示, 需要在`Markdown`中增加`copyright: true`的设置.

如果你觉得每次都要输入`copyright: true`很麻烦的话, 那么在`scaffolds/post.md`文件中添加：
```
---
title: {{ title }}
date: {{ date }}
tags:
categories:
copyright: true
---
```
这样每次`hexo new "你的内容"`之后, 生成的`.md`文件会自动把`copyright: `加到里面去.

## 隐藏网页底部powered By Hexo / 强力驱动

打开`themes/next/layout/_partials/footer.swig`,
将下面的内容
```
<div class="powered-by">{##}{{ __('footer.powered', '<a class="theme-link" target="_blank" href="https://hexo.io">Hexo</a>') }}{##}</div>
```
使用`<!-- <div>xxx</div> -->`隐藏之间的代码即可, 或者直接删除.

## 隐藏网页底部主题链接

打开`themes/next/layout/_partials/footer.swig`文件, 将`{% if theme.footer.theme.enable %}...{% endif %}`注释.

## 修改网页底部的桃心

打开`themes/next/layout/_partials/footer.swig`, 找到:
```
<span class="with-love">
  <i class="fa fa-{{ theme.footer.icon }}"></i>
</span>
```
然后在[图标库](https://fontawesome.com/icons?from=io) 找到自己喜欢的图标, 然后修改`<i class="fa fa-{{ theme.footer.icon }}"></i>`就可以了.

## 文章加密访问

打开`themes/next/layout/_partials/head.swig`文件, 在以下位置插入这样一段代码:
```
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误！');
                history.back();
            }
        }
    })();
</script>
```

然后在文章的`--- ---`中添加一个`password: password`, 后一个`password`为密码.

## 添加jiathis分享

 - JiaThis关闭了分享服务
    在`themes/next/_config.yml`中, 将`jiathis`的值改为`true`

    如果你想自定义样式的话, 打开`themes/next/layout/_partials/share/jiathis.swig`, 修改以下部分就可以了
    ```
    <div class="jiathis_style">
    <span class="jiathis_txt">分享到：</span>
    <a class="jiathis_button_fav">收藏夹</a>
    <a class="jiathis_button_copy">复制网址</a>
    <a class="jiathis_button_email">邮件</a>
    <a class="jiathis_button_weixin">微信</a>
    <a class="jiathis_button_qzone">QQ空间</a>
    <a class="jiathis_button_tqq">腾讯微博</a>
    <a class="jiathis_button_douban">豆瓣</a>
    <a class="jiathis_button_share">一键分享</a>
    ```

## 使用百度分享

在`themes/next/_config.yml`中, 添加或者修改字段`baidushare`, 值为`true`.

## 修改字体大小

打开`themes/next/source/css/_variables/base.styl`文件, 将`$font-size-base`改成`16px`, 如下所示:
```
$font-size-base = 16px
```

## 打赏

微信生成二维码, 支付宝生成二维码

命名要和`Reward`中的一样.

将图片移动到`themes/next/source/images/`下面, 修改文件`themes/next/_config.yml`中的字段:
```
# Reward
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: /images/wechatpay.jpg
alipay: /images/alipay.jpg
bitcoin: /images/bitcoin.png
```

## 修改打赏字体不闪动

修改文件`themes/next/source/css/_common/components/post/post-reward.styl`, 然后注释其中的函数`wechat:hover`和`alipay:hover`, 如下:
```
/* 注释文字闪动函数
 #wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
#alipay:hover p{
  animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
*/
```

## 阅读全文

 - 在文章中使用`<!--more-->`手动进行截断  
    这种方法可以根据文章的内容, 自己在合适的位置添加`<!--more-->`标签, 使用灵活, 也是`Hexo`推荐的方法.

 - 在文章中的`front-matter`中添加`description`, 并提供文章摘录  
    这种方式只会在首页列表中显示文章的摘要内容，进入文章详情后不会再显示。

 - 自动形成摘要，在主题配置文件中添加  
    默认截取的长度为 150 字符，可以根据需要自行设定
    ```
    auto_excerpt:
      enable: true
      length: 150
    ```

建议使用`<!-- more --`(即第一种方式), 除了可以精确控制需要显示的摘录内容以外, 这种方式也可以让`Hexo`中的插件更好的识别.


## 添加博客更新时间

修改博客主目录`./themes/next/layout/_macro/post.swig`文件, 在`<span class="post-time">...</span>`标签后添加:
```
{%if post.updated and post.updated > post.date%}
  <span class="post-updated">
    &nbsp; | &nbsp; {{ __('post.updated') }}
    <time itemprop="dateUpdated" datetime="{{ moment(post.updated).format() }}" content="{{ date(post.updated, config.date_format) }}">
      {{ date(post.updated, config.date_format) }}
    </time>
  </span>
{% endif %}
```

根据博客配置文件中的`language`参数修改对应的语言配置文件`./themes/next/languages/zh_Hans.yml`, 如果文件中有`updated`, 
修改值为`updated: 更新时间`, 如果没有`updated`, 则添加并赋值`updated: 更新时间`.

修改主题配置文件`./themes/next/_config.yml`, 增加一行:
`display_updated: true`

写文章的时候可以直接在文章开头设置更新时间`updated: 2018-05-06 00:00:00`, 没有这参数的话将会显示md文件的修改日期.

## 博客在chrome上打开过慢的解决办法

原因:
    加载`google`字体库导致

解决办法:
     - 将主题配置文件中的`font`字段的`host`值设为`host: //fonts.lug.ustc.edu.cn`, 默认使用的是`//fonts.lug.ustc.edu.cn`.


## Local Search(百度/谷歌/本地搜索)

添加`百度/谷歌/本地`自定义站点内容搜索

 - 安装`hexo-generator-searchdb`, 在站点的根目录下执行以下命令:
    ```
    npm install hexo-generator-search --save
    npm install hexo-generator-searchdb --save
    ```

 - 编辑`站点配置文件`, 新增以下内容到任意位置:
    ```
    search:
      path: search.xml
      field: post
      format: html
      limit: 10000
    ```

 - 编辑`主题配置文件`, 启用本地搜索功能:
    ```
    # Local search
    local_search:
      enable: true
    ```

## 公益404
在主题`source`目录下, 新建`404.html`页面, 内容如下:
```
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
  <link rel="stylesheet" type="text/css" href="https://qzone.qq.com/gy/404/style/404style.css">
</head>
<body>
  <script type="text/plain" src="http://www.qq.com/404/search_children.js"
          charset="utf-8" homePageUrl="/"
          homePageName="回到我的主页">
  </script>
  <script src="https://qzone.qq.com/gy/404/data.js" charset="utf-8"></script>
  <script src="https://qzone.qq.com/gy/404/page.js" charset="utf-8"></script>
</body>
</html>
```

## 添加菜单页面
 - 添加`关于`页面
`hexo new page about`

 - 添加`标签`页面
`hexo new page tags`

在`/source/tags/index.md`中添加内容:
```
---
title: tags
date: 2018-06-07 20:53:22
type: tags
---
```

 - 添加`分类`页面
`hexo new page categories`

 - 添加`归档`页面

 - 添加`自定义`页面
`hexo new page "guestbook"`

在主题配置文件中的`menu`中修改:
```
menu:
  home: / || home
  archives: /archives/ || archive
  tags: /tags/ || tags
  #categories: /categories/ || th
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  commonweal: /404/ || heartbeat
  about: /about/ || user
```


## 为代码片段添加全部选择按钮

需要[clipboard.min.js](https://raw.githubusercontent.com/zenorocha/clipboard.js/master/dist/clipboard.min.js)

保存到`themes\next\source\lib\zclip\clipboard.min.js`, 没有文件夹则创建.

新建文件`themes\next\source\js\src\custom.js`, 添加内容为:
```
//此函数用于创建复制按钮
function createCopyBtns() {
    var $codeArea = $("figure table");
    //查看页面是否具有代码区域，没有代码块则不创建 复制按钮
    if ($codeArea.length > 0) {
        //复制成功后将要干的事情
        function changeToSuccess(item) {
             $imgOK = $("#copyBtn").find("#imgSuccess");
                if ($imgOK.css("display") == "none") {
                    $imgOK.css({
                        opacity: 0,
                        display: "block"
                    });
                    $imgOK.animate({
                        opacity: 1
                    }, 1000);
                    setTimeout(function() {
                        $imgOK.animate({
                            opacity: 0
                        }, 2000);
                    }, 2000);
                    setTimeout(function() {
                        $imgOK.css("display", "none");
                    }, 4000);
                };
        };
        //创建 全局复制按钮，仅有一组。包含：复制按钮，复制成功响应按钮
        //值得注意的是：1.按钮默认隐藏，2.位置使用绝对位置 position: absolute; (position: fixed 也可以，需要修改代码)
        $(".post-body").before('<div id="copyBtn" style="opacity: 0; position: absolute;top:0px;display: none;line-height: 1; font-size:1.5em"><span id="imgCopy" ><i class="fa fa-paste fa-fw"></i></span><span id="imgSuccess" style="display: none;"><i class="fa fa-check-circle fa-fw" aria-hidden="true"></i></span>');
        //创建 复制 插件，绑定单机时间到 指定元素，支持JQuery
        var clipboard = new Clipboard('#copyBtn', {
            target: function() {
                //返回需要复制的元素内容
                return document.querySelector("[copyFlag]");
            },
            isSupported: function() {
                //支持复制内容
                return document.querySelector("[copyFlag]");
            }
        });
        //复制成功事件绑定
        clipboard.on('success',
            function(e) {
                //清除内容被选择状态
                e.clearSelection();
                changeToSuccess(e);
            });
        //复制失败绑定事件
        clipboard.on('error',
            function(e) {
                console.error('Action:', e.action);
                console.error('Trigger:', e.trigger);
            });
        //鼠标 在复制按钮上滑动和离开后渐变显示/隐藏效果
        $("#copyBtn").hover(
            function() {
                $(this).stop();
                $(this).css("opacity", 1);
            },
            function() {
                $(this).animate({
                    opacity: 0
                }, 2000);
            }
        );
    }
}
//感应鼠标是否在代码区
$("figure").hover(
    function() {
        //-------鼠标活动在代码块内
        //移除之前含有复制标志代码块的 copyFlag
        $("[copyFlag]").removeAttr("copyFlag");
        //在新的（当前鼠标所在代码区）代码块插入标志：copyFlag
        $(this).find(".code").attr("copyFlag", 1);
        //获取复制按钮
        $copyBtn = $("#copyBtn");
        if ($copyBtn.lenght != 0) {
            //获取到按钮的前提下进行一下操作
            //停止按钮动画效果
            //设置为 显示状态
            //修改 复制按钮 位置到 当前代码块开始部位
            //设置代码块 左侧位置
            $copyBtn.stop();
            $copyBtn.css("opacity", 0.8);
            $copyBtn.css("display", "block");
            $copyBtn.css("top", parseInt($copyBtn.css("top")) + $(this).offset().top - $copyBtn.offset().top + 3);
            $copyBtn.css("left", -$copyBtn.width() - 3);
        }
    },
    function() {
        //-------鼠标离开代码块
        //设置复制按钮可见度 2秒内到 0
        $("#copyBtn").animate({
            opacity: 0
        }, 2000);
    }
);
//页面载入完成后，创建复制按钮
$(document).ready(function() {
  createCopyBtns();
});
```

新建文件`themes\next\layout\_custom\custom.swig`, 内容为:
```
<script type="text/javascript" src="/lib/zclip/clipboard.min.js"></script>	
<script type="text/javascript" src="/js/src/custom.js"></script>
```

修改文件`themes\next\layout\_layout.swig`, 向`</body>`前一行插入文件引用:
```
{% include '_custom/custom.swig' %}
```

## 添加代码折叠功能

 - 添加折叠代码
`themes/next/source/js/src/post-details.js`, 添加如下代码:
```
$(document).ready(function(){
    $(document).on('click', '.fold_hider', function(){
        $('>.fold', this.parentNode).slideToggle();
        $('>:first', this).toggleClass('open');
    });
    //默认情况下折叠
    $("div.fold").css("display","none");
});
```

 - 添加自定义内建标签
 新建, `themes/next/scripts/tags.js`, 添加代码:
``` 
const rEscapeContent = /<escape(?:[^>]*)>([\s\S]*?)<\/escape>/g;
const placeholder = '\uFFFD';
const rPlaceholder = /(?:<|&lt;)\!--\uFFFD(\d+)--(?:>|&gt;)/g;
const cache = [];
function escapeContent(str) {
  return '<!--' + placeholder + (cache.push(str) - 1) + '-->';
}
hexo.extend.filter.register('before_post_render', function(data) {
  data.content = data.content.replace(rEscapeContent, function(match, content) {
    return escapeContent(content);
  });
  return data;
});
hexo.extend.filter.register('after_post_render', function(data) {
  data.content = data.content.replace(rPlaceholder, function() {
    return cache[arguments[1]];
  });
  return data;
});
```

再添加一个`fold.js`:
```
function fold (args, content) {
    var text = args[0];
    if(!text) text = "点击显示/隐藏";
    return '<div><div class="fold_hider"><div class="close hider_title">' + text + '</div></div><div class="fold">\n' + hexo.render.renderSync({text: content, engine: 'markdown'}) + '\n</div></div>';
}
hexo.extend.tag.register('fold', fold, {ends: true});

```

 - 添加自定义样式
`themes/next/source/css/_custom/custom.styl`, 追加:
```
.hider_title{
    font-family: "Microsoft Yahei";
    cursor: pointer;
}
.close:after{
    content: "▼";
}
.open:after{
    content: "▲";
}
```

 - 在需要隐藏的地方添加调用, (在md文件中的代码块的位置):
```
{% fold 点击显/隐内容 %}
something you want to fold, include code block.
{% endfold %}
```

## 加载图片

 两种方式，一种使用插件，一种不使用插件，修改配置文件`_config.yml`的`post_asset_folder`为`true`.

创建博客的命令: `hexo new [layout] <title>`, `layout`可以省略.
创建完成后会在`source/_post`目录生成一个同名的目录，把图片放在这个目录中.

### 不使用插件

`{% asset_img 这是图片的名称.jpg 这是对应的说明信息 %}`
