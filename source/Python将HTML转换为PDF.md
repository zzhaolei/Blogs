---
title: Python将HTML转换为PDF
copyright: true
date: 2018-05-06 09:56:02
tags:
  - Python
  - pdfkit
  - wkhtmltopdf
categories:
---

# Python将HTML转换为PDF

使用`pdfkit`库和`wkhtmltopdf`，
`pip install pdfkit wkhtmltopdf`linux中一般需要添加`sudo权限`.

`Windows`安装独立版本的`for Windows XP/2003 or later; standalone`。
Linux直接使用命令安装就行, `Fedora/CentOS`系`sudo yum install wkhtmltopdf`,
`Debian/Ubuntu`系`sudo apt install wkhtmltopdf`

```
import pdfkit
pdfkit.from_file(input_file, output_file)
```

## html转为pdf时的分页断行错误

在需要进行分页的块(如div)中添加样式`page-break-inside:avoid;`。

## 强制分页
想从html中的某个标签的位置进行强制分页, 添加样式`page-break-after:always;`

## 直接使用wkhtmltopdf的选项参数

使用字典的形式, `wkhtmltopdf`的选项作为字典的`key`, 删除选项前的`--`.
如果选项没有值, 使用`None`或者`False`.
对于可以有多个元素的选项来说(`cookie, custom-header, post, postfile, run-script, replace等`), 可以使用列表或者元组.

例如
```
options = {
    "page-size": "Letter",
    "cookie": [
        ('cookie-name', 'cookie-value')
    ],
    "no-outline": None
}
```

`wkhtmltopdf`的选项信息，可以参考这个网页`https://www.jianshu.com/p/4d65857ffe5e`, 对帮助信息的一个翻译.
或者使用`wkhtmltopdf -H`显示所有的帮助信息, 官方的文档也是这个.

