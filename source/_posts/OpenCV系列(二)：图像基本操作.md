---
title: OpenCV系列(二)：图像基本操作
date: 2018-10-12
tags: [OpenCV, Python]
---

# 主要内容

- 学习如何获取和修改像素值
- 学习如何获取图像属性
- 学习设获取图像区域
- 学习分割与合并图像

<!-- more -->

像素(Pixel)是构成图像的基本元素。每个像素的取值范围为0~255。对于灰度图，所有的像素只有一个通道，而对于彩色图，通常有三个通道，如BGR或者HSV。在前面我们提到，OpenCV中的通道顺序为BGR而不是常见的RGB，在这里我们通过实验来验证。

```Python
import numpy as np 
import matplotlib.pyplot as plt
import cv2

img_cv = cv2.imread("opencv-logo.png", 1)
img_normal = plt.imread("opencv-logo.png", 1)
plt.figure()
plt.subplot(1, 2, 1)
plt.imshow(img_cv)
plt.title("cv2")
plt.subplot(1, 2, 2)
plt.imshow(img_normal)
plt.title("matplotlib")
plt.show()
```

显示结果如下图所示：

<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/channel.png"  />

左图是通过cv2载入的图像，右图是通过matplotlib载入的图像，可以看到，两张图中红色和蓝色的位置刚好发生了对调。

## 获取和修改像素值

访问和修改像素值，常用的方式有两种，一种是通过矩阵直接操作，另一种是通过Numpy的函数来间接操作。通过矩阵操作比较容易理解，就是通过坐标来取出相应的元素，比如下面一段代码：

```Bash
>>> import numpy as np
>>> import cv2
>>> img = cv2.imread("opencv-logo.png", 1)
# 获取图像(100, 100)处三个通道的像素值
>>> px = img[100, 100]
>>> print(px)
[255 255 255]
# 获取图像(100, 100)处第一个通道(B通道)的像素值
>>> px_b = img[100, 100, 0]
>>> print(px_b)
255
# 修改图像(100, 100)处第一个通道(B通道)的像素值
>>> img[100, 100, 0] = 0
>>> print(img[100, 100, 0])
0
```

间接访问和修改像素，主要通过Numpy中的`array.item()`函数和`array.itemset()`函数来实现，具体用法如下：

```Bash
>>> import numpy as np
>>> import cv2
>>> img = cv2.imread("opencv-logo.png", 1)
# 获取图像(100, 100)处第一个通道(B通道)的像素值
>>> img.item(100, 100, 0)
255
# 修改图像(100, 100)处第一个通道(B通道)的像素值
>>> img.itemset((100, 100, 0), 0)
>>> img.item(100, 100, 0)
0
```

> **注意：** 当需要获取或者修改单个像素时，建议使用`array.item()`以及`array.itemset()`函数，因为Numpy是经过优化的库，执行效率更高。

## 获取图像属性

图像属性包括图像像素的行数、列数、通道数，像素的个数以及像素的数据类型，其分别可通过Numpy中的`array.shape`、`array.size`以及`array.dtype`三个属性来获取。

```Bash
>>> import numpy as np
>>> import cv2
>>> img = cv2.imread("opencv-logo.png", 1)
# 获取像素的行数、列数以及通道数
>>> rows, cols, chans = img.shape
>>> print(rows, cols, chans)
739 600 3
# 获取像素个数
>>> print(img.size)
1330200
# 获取像素的数据类型
>>> print(img.dtype)
uint8
```

## 设置ROI

获取图像区域是图像处理中的基本操作，常常用来得到ROI。ROI是Region of Interest的简称，指图像中我们感兴趣或者关注的区域。ROI在图像处理的很多场景如图像分割、目标检测等中都有广泛应用。通过如下操作，我们可以将图片中的”OpenCV“字样取出来。不难看出，设置ROI的操作就是Python中的切片操作。

```Python
import numpy as np 
import cv2

img = cv2.imread("opencv-logo.png", 1)
roi = img[570:, :, :]
```

<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/roi.png"  />

## 分割与合并图像通道

有时候我们可能需要取出图像中的某一个通道来对其进行单独的操作，然后，可能会重新将该通道合并到图像中，此时，就涉及到图像通道的分割与合并操作。在OpenCV中，提供了`cv2.split()`函数和`cv2.merge()`函数来完成该操作。

```Python
import numpy as np
import cv2

img = cv2.imread("opencv-logo.png", 1)
# 分割图像通道
b, g, r = cv2.split(img)
# 合并图像通道
img = cv2.merge((b, g, r))
```

<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/split_merge.png" />

### 参考资料

[OpenCV-Python Tutorials: Basic Operations on Images](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_core/py_basic_ops/py_basic_ops.html#basic-ops)