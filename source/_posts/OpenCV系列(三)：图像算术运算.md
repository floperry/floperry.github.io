---
title: OpenCV系列(三)：图像算术运算
date: 2018-10-14
tags: [OpenCV, Python]
mathjax: true
---

# 主要内容

- 学习图像中的相加操作，了解`np.add()`与`cv2.add()`函数的不同之处
- 学习图像融合操作以及`cv2.addWeighted()`函数

<!-- more -->

# 图像相加

当两个矩阵相加时，直接通过`np.add()`函数或者操作符`+`就可以实现。对于图片的相加，OpenCV中却有单独的函数`cv2.add()`。这两个函数有什么不同呢？我们首先来看一个例子。

```Python
import numpy as np
import cv2

img1 = cv2.imread("opencv-logo.png", 1)
img2 = cv2.imread("pure.png", 1)
img3 = np.add(img1, img2)
img4 = cv2.add(img1, img2)

cv2.imshow("img1", img1)
cv2.imshow("img2", img2)
cv2.imshow("img3", img3)
cv2.imshow("img4", img4)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/image_add.png" >

可以看到，`np.add()`和`cv2.add()`的结果完全不同。这主要是由于图像中像素的数据类型导致的。前面我们说过，图像中像素的数据类型为uint8。在OpenCV的官方文档中提到，Numpy中的相加是modulo operation(模除运算)，而OpenCV中的相加是saturated operation(饱和运算)。这是什么意思呢？我们来看下面这个例子。

```Bash
>>> x = np.uint8([250])
>>> y = np.uint8([100])
>>> print(np.add(x, y))
[94]
>>> print(cv2.add(x, y))
[[255]]
```

简单总结就是，对`np.add()`，当相加后的结果超出数据类型的最大值时，会对结果做模除操作，即`A+B=(A+B)%2^8`；对`cv2.add()`，当相加后的结果超出数据类型最大值时，结果直接取最大值，即`A+B=min(A+B,2^8-1)`。  
回头再看第一个例子中两张图片相加的结果就比较容易理解了。对于img1，只存在4种像素值，即[255, 255, 255]，[255, 0, 0]，[0, 255, 0]，[0, 0, 255]，[0, 0, 0]，分别对应图中的白色、蓝色、绿色、红色以及白色。当img2采用`np.add()`与img1相加时，与255相加等同于原像素值-1，与0相加等同于原像素值，因此，相加后的结果与img2相差不大；当img2采用`cv2.add()`与img1相加时，与255相加仍等于255，与0相加等同于原像素值，因此相加后，img1中白色部分依旧为白色，黑色部分变成img2中对应位置的颜色，而蓝色、绿色和红色部分由于其他两个通道上的值发生了变化，因此颜色也会相应的发生变化。  
> **注意：** 当两张图片相加时，需要保证两张图片具有相同的shape。当然也可以通过Python的广播机制，将一张图片与一个标量相加。

# 图像融合

除了上面介绍的两种图像相加操作，还有一种称为带权相加，这种相加方式常常在图像融合技术中应用。带权相加按以下公式进行: 

$$dst=saturate(src_{1}\cdot\alpha+src_{2}\cdot\beta+\gamma)$$

在OpenCV中，函数`cv2.addWeighted()`实现图像的带权相加。该函数的语法格式为：`cv2.addWeighted(src1, alpha, src2, beta, gamma)`，其中，src1和src2需要保持相同的shape，alpha、beta以及gamma分别为src1和src2的权重以及偏移量。通过一段程序来具体了解一下该函数的使用。

```Python
import numpy as np
import cv2

img1 = cv2.imread("img1.png", 1)
img2 = cv2.imread("img2.png", 1)
dst = cv2.addWeighted(img1, 0.4, img2, 0.6, 0)

cv2.imshow("img1", img1)
cv2.imshow("img2", img2)
cv2.imshow("dst", dst)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

图像混合后的效果如下所示：
<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/blending_img1.png" width = "700" height = "300" >
<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/blending_img2.png" width = "700" height = "300" >
<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/blending_dst.png" width = "700" height = "300" >




