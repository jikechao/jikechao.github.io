---
layout:     post
title:      "tensorflow、pytorch环境安装"
subtitle:   " \"gpu版本环境安装\""
date:       2020-06-22 19：23
author:     "Jack-C"
header-img: "img/post-bg-DL.jpg"
catalog: true
tags:
    - DL
    - GPU
    - 运维
---

# TensorFlow

## 1、操作流程

1. 使用Anaconda创建一个虚拟环境

2. 安装tensorflow-gpu 包

3. 安装cuda  ToolKit  + cudnn

   - 查看需要安装的CUDA+cuDNN版本（与tf一致），下载
   - 将cudnn压缩包中文件，放到CUDA文件的根目录下

   - 添加cuda到环境变量

     ![1592824652459](../_post_assets/1592824652459.png)

   - nvcc -V # 验证环境配置

   - 测试：

     ```python
     import os
     os.environ["CUDA_VISIBLE_DEVICES"] = "0"  # -1代表cpu,0:第一块GPU，1:第二块GPU...
     
     import tensorflow as tf
     
     print(tf.__version__)
     print(tf.test.gpu_device_name())  # GPU的名字
     sess = tf.Session(config=tf.ConfigProto(log_device_placement=True))
     ```



## 2、注意事项

- keras 只是个前端接口，必须安装后端（如：Tensorflow）
- 只要配置好环境，安装了TG-gpu，默认使用gpu, 如果无gpu则会自动使用cpu。可手动修改os.environ["CUDA_VISIBLE_DEVICES"] = "0"  使用的设置
- tensorflow  + cuda + cudnn 版本必须一致。<a href= 'https://tensorflow.google.cn/install/source'>看这里</a>
- Tensorflow 2 和Tensorflow不兼容，建议下载到不同的环境中（使用conda创建）。



## 3、使用Tensorflow2 运行tf

```python
import tensorflow.compat.v1 as tf
```





## 4. Ubuntu 安装cudnn

* 下载地址：https://developer.nvidia.com/rdp/cudnn-download

* 选择对应cuda 的版本，下载的是一个solitairetheme8文件，可以上传到服务器上

* 执行下面步骤

  ```shell
  cp cudnn-9.0-linux-x64-v7.3.1.20.solitairetheme8 cudnn-9.0-linux-x64-v7.3.1.20.tgz
  tzr -zxvf cudnn-9.0-linux-x64-v7.3.1.20.tgz
  sudo cp cuda/include/cudnn.h /usr/local/cuda/include
  sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
  sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
  ```

  

# pytorch

> 对应的方式和与cuda的对应版本详见网址： https://pytorch.org/get-started/previous-versions/