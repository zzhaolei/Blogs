---
title: Jenkins+Python+GitLab持续集成
copyright: true
date: 2018-05-06 09:53:08
tags:
  - jenkins
  - Python
  - GitLab
categories:
---

# 创建任务
登录`Jenkins`，点击左侧列表的`新建`选项。输入任务名称，选择`构建一个自由风格的软件项目`，点击`确定`。

# 配置
在`任务配置`界面，可以设置`General标签`中的`丢弃旧的构建`选项，设置`保持构建的天数`和`保持构建的最大个数`。

点击`源码管理`标签，选择`git`，在`Repository URL`中输入Git仓库的链接。

如果是HTTP链接，选择`Credentials`一行的`Add > Jenkins`,在`Kind`一行下拉选择`Username with password`,在下方`Username`和`Password`框中输入用户名和密码。

~~使用docker jenkins容器里面的ssh-key。~~
直接使用服务器的`ssh key`就可以了。如果指定`Path to key`需要使用`docker`中的`ssh key`。如果使用`Source files`那么需要使用`docker`容器中的`key`, 因为`Source files`的相对路径不是固化到本地的路径, 而是`docker`中的路径.


如果是`SSH`链接，选择`SSH Username with private key`,在`Username`框中输入用户名，`Private Key	`选择为`Enter directly`,在显示的`key`框中输入私钥。（生成秘钥`ssh-keygen -t rsa -C "email.com"`,生成的文件在`home`目录下的`.ssh`目录下,使用命令`cat id_rsa`查看私钥，复制到`key`框中）。点击最下方的`Add`按钮，保存设置。

在`Credentials`的下拉选项中选择刚刚创建`Username`。

# 使用定时任务
在`构建触发器`标签里选择`Poll SCM`,输入`H/2 * * * *`, 最后点击`保存`。
这是一个定时任务，每隔2分钟查询一次代码，如果有更新，就执行任务。

# 使用webhook
使用webhook比定时任务更好，webhook只有在push或指定操作时，jenkins会拉取代码，不用定时访问，耗费资源。

需要安装的插件`Gitlab Hook、GitLab、Gitlab Authentication、Build Authorization Token Root`
配置`job`，在`构建触发器`中勾选`Build when a change is pushed to GitLab. GitLab CI Service URL: http://服务器IP:8080/project/demo` ，`URL`后面是回调地址，再点击`高级`，再`Secret token`行，点击`Generate`生成`token`。

在`GitLab`服务器上的相对应的项目的`Settings`里面的`Integrations`中，配置`webhook`,输入在jenkins中获取的`URL`,和`token`，可以根据需要勾选事件，开启SSL，点击`Add webhook`。会在`Add webhook`按钮下生成一个`webhook`，点击`test`,会弹出勾选的事件，选择相应的事件，会在页顶弹出`Hook executed successfully: HTTP 200`,到此，`webhook`配置完成。

### 注意：在脚本中使用`docker exec`执行容器中的命令是, 不要指定`-it`, `-t`是指定`tty`, 而`jenkins`中没有`tty`, 只需要指定`-i`即可.

# 1 将代码部署到远程服务器

不需要使用容器中的ssh-key。
在jenkins服务器上生成key`ssh-keygen -t rsa -C "email.com"`，
使用`ssh-copy-id -i ~/.ssh/id_rsa.pub user@ip`(如果端口不是22，可以添加参数`-p port`进行修改，`port`为端口数值), 将生成的公钥保存在目标服务器。这样可以免密码登录。

安装`Publish over SSH`插件，在`系统管理`-->`系统设置`里面找到`Publish over SSH`。
配置`Jenkins SSH Key`，如果key在生成时设置了密码，就在`Passphrase`里输入密码，没有则忽略。
复制私钥，粘贴进`Key`里面。

配置`SSH Servers`,点击`增加`。`Name`为SSH的名称，`Hostname`是远程服务器的IP地址，`Username`是登录的用户名，`Remote Directory`是远程服务器的目录，必须是真实存在的，插件不会自动创建。

点击`Test Configuration`测试配置是否成功。成功会显示`Success`。

# 1.1远程部署任务

再创建一个构建任务，当有触发任务时，构建任务运行。

还是点击`新建`，输入一个任务名称，不点击`构建一个自由风格的软件项目`,在最下面的复制框中，输入刚刚创建的触发任务名称。
点击`确定`。如果输入的任务名不存在，会报错，并让你重新输入一次。

在`构建触发器`中选择`Build after other projects are built`，在其他项目建成后进行构建，`Projects to watch`选择第一个任务，可能会报错`No such project ‘gol’. Did you mean ‘golismero’?`，不用理会，如果是单个任务，可以把最后面的`, `逗号和空格删除，就不会报错了，不删也没有影响。
勾选下面的`Trigger only if build is stable`，只有在构建稳定时才触发。

其他设置不变，`构建后操作`中`增加构建后操作步骤`，在弹出的选项中选择`Send build artifacts SSH`,输入`SSH Server Name`(是远程主机IP)，然后在`Transfers`中的`Exec command`中输入脚本命令，其他选项暂时不用填写。

`Source files`是相对于`workspace`下项目目录的目录, 是一个相对目录, 如果在`workspace`目录, 即`jenkins`的工作目录, 那么需要填写`**/`, 后面可以跟工作目录下的文件或文件夹.

`Remove prefix`文件复制时要过滤的目录.

`Remote directory`文件得到到远程机上的目录，此目录是相对于`SSH Server`中的`Remote directory`的，如果不存在将会自动创建。

保存，即可看到触发任务和构建任务。


还有另一种方式可以部署

# 2 SSH插件部署
这种方式需要安装`SSH`插件。

点击`Credentials`-->`System`-->`Add domain`，在`Domain Name`中输入名称，`Description`描述，可写可不写，`Specification`保持默认为`Add`，点击`ok`。在新页面点击左上的`Add Credentials`，输入远程登录的`Username`和`Password`，点击`OK`。
点击`系统管理`-->`系统设置`，找到`SSH remote hosts`，配置`SSH site`，点击`增加`，输入`Hostname`主机IP，`Port`远程连接端口（一般为22），`Credentials`选择刚刚创建`Domain`，和`Username`相同，点击`Check connection`，显示`Successfull connection`，成功，点击`保存`。

# 2.1 远程部署任务
基本和`1.1`相同，但是这个是在`构建`中`增加构建步骤`，在弹出的选项中选择`Execute shell script on remote host using ssh`，默认会自动填写`SSH site`（如果没有填写，点击输入框看看有没有，如果没有可能是前面出现错误），可能会有红字报错`SSH Site not specified`，不用理。在`Command`输入框中输入`shell`脚本。`保存`即可。

这种方式没有第一种构建的快。

# 配置邮件

可以安装`Email Extension Plugin`插件，扩展邮箱功能。

点击左侧的`系统管理`，在`管理Jenkins`列点击`系统设置`，在新页面找到`Jenkins Location`,在`系统管理员邮件地址`栏中输入发件人，格式为`标题<email.com>`,再找到`邮件通知`，点击`高级`按钮，添加`SMTP服务器`,以及邮件后缀(`@xxx.com`)，使用`SMTP`验证的用户名为完整的邮箱账号，密码是授权码。（腾讯企业邮箱如果绑定了微信，在设置-微信绑定-客户端专用密码，生成授权码，保存设置。）勾选使用`SSL`协议，`SMTP`端口是`465`, `Reply-To Address`填写收件人地址。


# 高级版邮件通知

需要安装`Email Extension`插件，安装好后，点击`系统管理`-->`系统设置`找到`Extended E-mail Notification`，配置`SMTP server`服务器地，格式为`smtp.exmail.qq.com`(这里用腾讯企业邮箱作为例子).

`Default user E-mail suffix`设置邮箱后缀，就是@及后面的字符`@xxx.com`.

`Default Content Type`默认发送的内容类型选择`HTML/(text/html)`.

`Default Recipients`默认收件人，可以设置多个收件人，中间用`, `隔开，英文状态下的符号.

`Default Subject`默认主题不用改，也可以在最前面加一些文字，比如`构建通知`.

`Default Content`这里面是默认模板，可以在网上找模板更改，也可以自己写，html类型。

点击上面的`高级`按钮，勾选`Use SMTP Authentication`，输入`Username`和`Password`，`Password`是客户端授权密码要和`配置邮件`中的授权码一样，勾选`SSL`，点击`保存`。


为单个的`Job`增加`构建后步骤`, 
选择`Editable Email Notification`, 在`Project Recipient List`收件人列表中输入收件人邮箱, 多个可以用`, `分割, 想使用全局默认配置的话, 可以使用`$DEFAULT_RECIPIENTS`. 

`Project Reply-To List`允许回复人列表, 默认值`$DEFAULT_REPLYTO`。

`Content Type`文档类型, 可以设置`HTML`。

`Attach Build Log`发送的邮件是否包含日志。

点击`Advanced Settings`, 在`Triggers`中, 删除`Sent To`默认的`Developer List`, 点击`Add`添加`Recipient List`, 保存完成。


