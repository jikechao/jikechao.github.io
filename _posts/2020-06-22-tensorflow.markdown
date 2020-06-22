---
layout:     post
title:      "tensorflow环境安装"
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

