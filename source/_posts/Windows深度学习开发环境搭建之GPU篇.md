---
title: Windows深度学习开发环境搭建之GPU篇
tags: [机器学习, Python]
---

入坑深度学习已有足足一年，一直在matlab上勤勤恳恳，如今tensorflow大火，作为深度学习微不足道的一员，不了解tensorflow实在说不过去。恰逢今日周六，有足够的时间用来折腾，果断用来搭建深度学习的tensorflow开发环境。受限于CPU的计算能力，首选GPU作为计算单元。
<!-- more -->

1、**安装Python环境**

建议直接安装Anaconda，其优势在于内部集成Python环境，而且可以方便的对Python的各种包进行安装和管理。对于Python的版本选择，建议选择3.5，可以到[Anaconda archive](https://repo.continuum.io/archive/index.html)下载合适的版本。Python 3.5 对应的Anaconda最新版本为4.2.0。强烈推荐从清华大学开源软件镜像站进行[下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)，下载速度快且稳定。
建议同时修改包管理镜像为国内源

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

2、**升级pip**

为了防止包安装过程中反复出现警告，先对pip进行升级

```
python -m pip install -U pip
```

顺便，建议把软件获取源一同改掉以保证以后对软件的下载和升级能够顺利进行。首先，在 C:\Users\用户名 目录中新建pip目录，然后新建pip.ini文件，其内容如下

```
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

3、**安装tensorflow**

```
pip install --upgrade --ignore-installed tensorflow-gpu
```

4、**安装GPU支持包**

首先从[CUDA Toolkit Download](https://developer.nvidia.com/cuda-downloads)找到合适的版本下载并安装。然后，从[NVIDIA cuDNN](https://developer.nvidia.com/cudnn)下载cuDNN 5.1(据说6.0或以上版本目前tensorflow不支持)。将下载的cuDNN解压缩，并将bin文件夹下的cudnn64_5.dll文件放到 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin 目录下。

5、**测试tensorflow**

```
$ python
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))
```

测试结果为 b`Hello, TensorFlow! 则安装成功。若出现 ImportError: No module named '_pywrap_tensorflow' Failed to load the native TensorFlow runtime，则需要下载安装[Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53587)。

6、**安装keras**

```
pip install keras
```

在 import keras 过程中可能出现 AttributeError: module 'pandas' has no attribute 'computation'， 此时需要更新dask

```
pip install dask --upgrade
```





