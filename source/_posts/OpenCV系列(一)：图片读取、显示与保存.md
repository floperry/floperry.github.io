---
title: OpenCV系列(一)：图片读取、显示及保存
date: 2018-10-10
tags: [OpenCV, Python]
---

# 主要内容

- 学习如何读取图片、显示图片以及保存图片
- 学习使用函数：`cv2.imread()`、`cv2.imshow()`以及`cv2.imwrite()`

<!-- more -->

## 读取图片

在OpenCV中，主要使用`cv2.imread()`函数来读取图片。在读取图片时，应该指明图片所在位置的相对路径或绝对路径。当图片由于文件不存在、权限问题、格式不支持等原因导致无法读取时，该函数会返回一个空数组。OpenCV支持的图片格式包括：

- Windows bitmaps -- (.bmp, .dib ) 
- JPEG files -- (.jpeg, .jpg, .jpe) 
- JPEG2000 files -- (.jp2)
- Portable Network Graphics -- (.png  )
- Portable image format -- (.pbm, .pgm, .ppm)
- Sun rasters - (.sr, .ras)
- TIFF files - (.tiff, .tif)

cv2.imread()函数的语法格式为：`cv2.imread(filename[, flags])`  
其中，`flags`用于指定读取图像的颜色类型：

- `cv2.IMREAD_COLOR`: 载入彩色图像（默认）
- `cv2.IMREAD_GRAYSCALE`: 载入灰度图像  
- `cv2.IMREAD_UNCHANGED`: 载入包含alpha通道的原始图像

除了以上三种表示方法，我们也可以用1、0、-1来分别表示以上三个参数。
> **注意:** 当读取彩色图像时，图像是以B、G、R的通道顺序存储的，而不是常见的R、G、B顺序。  

## 显示图片

在OpenCV中，主要使用`cv2.imshow()`函数来显示图片。cv2.imshow()函数的语法格式为：`cv2.imshow(winname, img)`。其中，`winname`是显示图像窗口的名称，`img`是要显示的图像。  
有时我们可能会事先创建一个窗口，但并不及时在该窗口上显示图像，或者我们需要指明是否需要重新裁剪。这时候可以使用`cv2.namedWindow()`函数。该函数的语法格式为：`cv2.namedWindow(winname[, flag])`，其中，`flag`用于指定窗口的大小，默认为`cv2.WINDOW_AUTOSIZE`。当设定为`cv2.WINDOW_NORMAL`时，我们可以在后续对该窗口进行裁剪。

## 保存图片

在OpenCV中，主要使用`cv2.imwrite()`函数来保存图片。cv2.imwrite()函数的语法格式为：`cv2.imwrite(filename, img)`。其中，第一个参数是保存的文件名，其文件格式与读取图片时支持的文件格式一致，第二个参数是待保存的图片。

## 程序示例

以下通过一段完整的程序说明如何使用OpenCV来进行图片的读取、显示和保存。

```Python
import numpy as np
import cv2

# Load a color image in grayscale
img = cv2.imread('opencv-logo.png', 0)
# Create a new window
cv2.namedWindow('image', cv2.WINDOW_NORMAL)
# Display an image
cv2.imshow('image', img)
# Wait for keyboard events
k = cv2.waitKey(0)
if k == 27:    # wait for ESC key to exit
    cv2.destroyAllWindows()    # destroy all windows
elif k == ord('s'):    # wait for 's' key to save and exit
    cv2.imwrite('opencv-graylogo.png', img)    # save image
    cv2.destroyAllWindows()
```

原图和保存的图片分别如下所示：  

<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/opencv-logo.png" width = "400" height = "300" class="img-3"/>
<img src="https://raw.githubusercontent.com/floperry/floperry.github.io/hexo/source/images/opencv-graylogo.png" width = "400" height = "300" class="img-3"/>

程序示例中涉及到两个新的函数：

- `cv2.waitKey()`：等待键盘响应函数。在等待键盘响应时间内，如果按下某个键，则返回其ASCII值。如果没有键盘响应，返回-1。当该函数的参数设置为0时，表示无限期等待键盘输入。
> **注意:** 当使用64位机器时，我们需要使用`cv2.waikKey() & 0xFF`来保留低8位和32位机器一致。
- `cv2.destroyAllWindows()`：删除所有的窗口。当需要删除指定的窗口时，可以使用`cv2.destroyWindow(winname)`函数。

### 参考资料

[OpenCV-Python Tutorials: Getting Started with Images](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_gui/py_image_display/py_image_display.html#py-display-image)
