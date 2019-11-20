---
title: Python识别图片
copyright: true
date: 2018-05-06 09:58:10
tags:
  - Python
  - opencv-python
  - Pillow
  - pytesseract
categories:
---

# Python识别图片

Python使用opencv、Pillow识别图片。

需要用到系统级需要安装tesseract-ocr，
python库需要`opencv-python`, `Pillow`, `pytesseract`

步骤一般为下载图片-->读取图片-->将图片灰度化-->二值化-->去除图片中的干扰线-->识别。


## 从网络中直接获取图片并读取

示例
```
import requests
import cv2
from io import BytesIO
import pytesseract
import numpy
from PIL import Image

image = Image.open(BytesIO(response.content))
cv_image = cv2.cvtColor(numpy.asarray(image), cv2.COLOR_RGB2BGR)
cv2.imwrite('./test.png', cv_image)
print pytesseract.image_to_string(cv_image)
```

```
使用`requests`库，发送请求

`content`读取出来的是Byte类型的数据
`response = requests.get(url).content`

使用Pillow的PIL从response中读取图片
`image = Image.open(BytesIO(response))`

将PIL读取的图片转为opencv支持的Image格式,COLOR_RGB2BGR转为灰度
`cv_image = cv2.cvtColor(numpy.asarray(image), cv2.COLOR_RGB2BGR)`
```

## opencv从本地读取图片

`image = cv2.imread(path)`

## opencv保存图片到本地

`cv2.imwrite(F:/images',image)`


