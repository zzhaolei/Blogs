---
title: Python生成word
copyright: true
date: 2018-05-06 09:57:41
tags:
  - Python
  - word
categories:
---

# Python生成word

使用`python-docx-template`库, 将`html`转为`word`。
`python-docx-template`可以使用类似`jinja2`的模板语法。
依赖`docx`库, 安装`pip install docxtpl docx`

`docxtpl`是`python-docx-template`的引擎。

`GitHub`主页`https://github.com/elapouya/python-docx-template`，上面有一些`docx`文件的模板使用示例，
和`Python`文件的示例。

个人fork的主页`https://github.com/zZhaoLei/python-docx-template`。


使用示例, 和使用`jinja2`替换的方式差不多。
```
from docxtpl import DocxTemplate
from docx.shared import Mm

doc = DocxTemplate("my_word_template.docx")
context = { 'company_name' : "World company" }
doc.render(context)
doc.save("generated_doc.docx")
```

`Mm`是毫米, 官方规定`for height and width you have to use millimeters (Mm), inches or points(Pt) class`

## 图片的添加

使用`InlineImage`

```
from docxtpl import DocxTemplate, InlineImage
from docx.shared import Mm

doc = DocxTemplate("my_word_template.docx")

context = {
    'image': InlineImage(doc, path, width=Mm(30))
}
doc.render(context)
doc.save("generated_doc.docx")
```

`path`可以是路径, 也可以是文件对象, `width`可以设置宽度或用`heigth`设置高度


## 使用xml生成word

先使用微软的office`新建空白文档`, 调整模板, 设置样式等.

最后另存为`xml`格式的文件, 使用`sublime`进行格式化, 需要安装`xml`格式化插件`Indent XML`

安装`sublime`后, 使用`ctrl+~`调出命令, `Sublime 3`粘贴
```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

`sublime 2`粘贴
```
import urllib2,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```

r然后通过`Preferences-->Package Control-->Install Package`, 在弹出的框中输入`Indent XML`, 点击就会自动安装.

使用`Esc`键可以关闭安装窗口.

在`Selection-->Format`中选择`Indent XML`, 保存.

现在可以查找想要进行模板替换的位置了, 使用`jinja2`的模板语法。

`Python`代码示例

```
# -*- coding:utf-8 -*-
from __future__ import unicode_literals
import jinja2

path = 'C:/Users/lei/Desktop/project/export/doc/'
fsl = jinja2.FileSystemLoader(searchpath=path)

env = jinja2.Environment(loader=fsl)
tpl = env.get_template("test.xml")

data = {
    'name': '这是一个测试',
    'ip': '10.10.10.66',
    'status': '正在进行中......',
    'start_time': '2018-3-28',
    'end_time': '2018-3-29',
    'time': '2018-3-29 123213212321',
    'num': '10000000'
}
with open(path + "test.doc", 'w') as f:
    f.write(tpl.render(data = data).encode('utf-8'))
```

我这里直接在word中新建的`空白文档`, 修改后直接保存为了`Word XML`, 不能写入为`docx`, 只能写入为`doc`.

`docx`是有目录结构的压缩包, `doc`没有目录结构.


