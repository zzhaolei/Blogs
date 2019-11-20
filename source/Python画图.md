---
title: Python画图
copyright: true
date: 2018-05-06 09:55:33
tags:
  - Python
  - Echarts
  - pyecharts-snapshot
  - pyecharts
categories:
---

# `Python`画图

## 使用`Echarts`

`Echarts`是百度开源的`js`绘图工具, 
`pyecharts`是`Echarts`的封装, 安装`pip install pyecharts`.

`pyecharts`只能将绘好的`js`保存为`html`, 可以使用`pyecharts-snapshot`将生成的`html`转为`pdf`或`png`或`git`.

官方文档`http://pyecharts.org/#/zh-cn/prepare`

`pyecharts-snapshot`依赖于`phantomjs`, Windows安装可以在官网下载`zip`包，解压后，将`C:\\**\bin\`目录添加到环境变量的`Path`中.

安装`pip install pyecharts-snapshot`

### 饼图
使用方式
```
# -*- coding:utf-8 -*-
from __future__ import unicode_literals
from pyecharts import Pie
from pyecharts_snapshot.main import make_a_snapshot
from PIL import Image
import os
import sys

savety = [u'危险', u'比较危险', u'比较安全', u'安全']
savety_colors = [u'#df0000', u'#ff5f5a', u'#37baf1', u'#10ae50']
savety_x = [6, 1, 2, 3]

# 标题居中
pie = Pie()
pie.add(
    # title
    '',
    # label
    savety,
    # 数值，比例
    savety_x,
    # 显示label
    is_label_show=True,
    # 不显示颜色提示的意思
    is_legend_show=False,
    # 是否高亮显示标签
    is_label_emphasis=False,
    # label字体size
    label_text_size=25,
    # 随机颜色
    # is_random=True,
    # 自定义标签颜色
    label_color=savety_colors,
    # 右侧工具箱
    is_toolbox_show=False
)

# 将生成的图片保存为html
pie.render()
# 将标准输出的信息，重定向到文件中
current = sys.stdout
f = open('temp', 'w')
sys.stdout = f
# 将html保存为pdf或png
make_a_snapshot('render.html', 'test.png')
# 还原标准输出
sys.stdout = current
f.close()
os.remove('render.html')
os.remove('temp')

img = Image.open('test.png')
w, h = img.size
img = img.resize((int(w*0.3), int(h*0.3)), Image.ANTIALIAS)
img.save('hhh.png')
os.remove('test.png')
```

使用`make_a_snapshot`生成的图片是屏幕的截图, 文件大小很大.
使用`PIL`对图片进行一个缩小, 如果直接使用`resize`命令, 会对图片进行扭曲压缩,不是等比缩放的,
对图片的宽和高都和一个固定值相乘,可以实现等比的缩放.
`Image.ANTIALIAS`是对图片进行高清的缩放.


### 柱状图

示例
```
# -*- coding:utf-8 -*-
from __future__ import unicode_literals
from pyecharts import  Bar

attr = ['Cifs', 'DNS', 'FTP', 'RPC', 'SMB']

info =[10, 2, 2, 18, '']
low = [6, 5, 5, 1, 1]
medium = [7, 8, 1, '', 6]
high = [1, 8, 1, '', 2]
urgent = ['', 1, 2, '', '']
colors = ['#10ae50', '#37baf1', '#ffa92e', '#ff605a', '#df0000']

bar = Bar('test')

# 全局配置项要在最后一个 add() 上设置，否侧设置会被冲刷掉。
# 交换x y轴
# is_convert=True
# y轴字体颜色，默认是黑色
# yaxis_label_textcolor='#00000'

# add(name, x_axis, y_axis,
#     is_stack=False,
#     bar_category_gap='20%', **kwargs)

bar.add('信息', attr, info, is_label_show=True, label_pos='inside', is_stack=True)
bar.add('低危', attr, low, is_label_show=True, label_pos='inside', is_stack=True)
bar.add('中危', attr, medium, is_label_show=True, label_pos='inside', is_stack=True)
bar.add('高危', attr, high, is_label_show=True, label_pos='inside', is_stack=True)
bar.add('紧急', attr, urgent, is_stack=True,
        # 网格线
        is_splitline_show=False,
        label_pos='inside',
        # 是否高亮显示标签
        # is_label_emphasis=False,
        # 高亮标签字体颜色
        # label_emphasis_textcolor='red',
        # 高亮标签的位置
        label_emphasis_pos='inside',
        # 标签数据
        is_label_show=True,
        # 标签颜色自定义
        label_color=colors,
        # 标签字体颜色
        label_text_color='#000',
        # 标签字体大小
        label_text_size=15,
        # x/y交换
        is_convert=True,
        # y轴刻度标签
        # yaxis_interval=0,
        # 是否显示y轴
        # is_yaxis_show=False,
        # y轴反向显示
        is_yaxis_inverse=True,
        # 不显示工具箱
        is_toolbox_show=False
        )
bar.render()
```


## 使用`matplotlib`

`matplotlib`使用的比较广泛, 有很多功能都还没有研究到.

安装`pip install matplotlib`

示例
```
# -*- coding: utf-8 -*-
from matplotlib import mlab, font_manager as fm
from matplotlib import pyplot as plt


savety = [u'危险', u'比较危险', u'比较安全', u'安全']
savety_colors = [u'#df0000', u'#ff5f5a', u'#37baf1', u'#10ae50']
labels = [u'信息', u'低风险', u'中风险', u'高风险', u'紧急']
savety_x = [6, 1, 1, 1]

# matplotlib显示中文的问题
plt.rcParams['font.sans-serif'] = ['SimHei']
# 显示负号
plt.rcParams['axes.unicode_minus'] = False
# 设置为圆形，不设置就是椭圆的
plt.axis('equal')
# 画饼图（数据，数据对应的标签，百分数保留两位小数点）
# patches是饼图的返回值，texts是饼外的文本，autotexts是饼内的文本
pathches, texts, autotexts = plt.pie(savety_x,
                                     # 饼图的颜色
                                     colors=savety_colors,
                                     # 标签名
                                     labels=savety,
                                     # 小数
                                     autopct='%1.1f%%',
                                     # 角度
                                     startangle=90,
                                     )

# 颜色提示, loc设置legend的位置，包括'upper right', 'upper left', 'lower right', 'lower left'
# bbox_to_anchor: 表示legend距离图形之间的距离，当出现图形与legend重叠时，可使用bbox_to_anchor进行调整legend的位置
plt.legend(loc='upper right', bbox_to_anchor=(1.1, 1.1))
# 一定要放在plt.show()之前
plt.savefig("PieChart.jpg")
plt.show()
plt.close()

```

