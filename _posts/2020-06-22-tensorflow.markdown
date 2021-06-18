---
layout:     post
title:      "tensorflow、pytorch等环境安装"
subtitle:   " \"gpu版本环境安装\""
date:       2020-06-22 19：23
author:     "Jack-C"
header-img: "img/post-bg-DL.jpg"
catalog: true
tags:
    - DL
    - GPU
    - 运维
typora-root-url: .
---

# TensorFlow

> cuda+cudnn+tensorflow：三者版本要兼容才可！

## 1、操作流程

1. 使用Anaconda创建一个虚拟环境

2. 安装tensorflow-gpu 包

3. 安装cuda  ToolKit  + cudnn

   - 查看需要安装的CUDA+cuDNN版本（与tf一致），下载
   
- 将cudnn压缩包中文件，放到CUDA文件的根目录下
  
- 添加cuda到环境变量
  
- ![image-20210531211552203](/../img/2020-06-22-tensorflow/image-20210531211552203.png)
  
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





## Caffe2

> 官方安装方法 ： https://caffe2.ai/docs/getting-started.html?platform=ubuntu&configuration=prebuilt

神奇，竟然安装的是pytorch包。然后就能用caffe2了，这谁能想到！！



## ONNX

> 好像只要装onnx和onnxruntime两个包就行

```shell
conda install onnx
conda install onnxruntime
```



## Keras

> keras 只是一个封装好的API，后端还是要安装TensorFlow、等框架，注意要和Keras的版本和TensorFlow要匹配！





----

# 源码安装TensorFlow

> 由于需要使用Gcov/Lcov 收集TensorFlow的关于C的coverage，所以需要编译安装TF

参考文献： [官网安装操作]([从源代码构建  | TensorFlow (google.cn)](https://tensorflow.google.cn/install/source?hl=zh-cn)) 、   [从源代码编译安装tensorflow_JohnJim的博客-CSDN博客](https://blog.csdn.net/JohnJim0/article/details/103036106)





## 特别注意：

![image-20210531211226646](/../img/2020-06-22-tensorflow/image-20210531211226646.png)

### 注意点二：

使用的numpy版本不要大于1.19，

 `pip install numpy==1.18.5 ` is recommended.







## 源码安装PyTorch(包含caffe2)

鸣谢：[安装教程](https://www.zdaiot.com/MLFrameworks/Pytorch/Pytorch%E7%BC%96%E8%AF%91/)





