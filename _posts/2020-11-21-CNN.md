---
layout:     post
title:      "CNN"
subtitle:   " \"花里胡哨的卷积\""
date:       2020-11-21 19：26
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - ML
    - DL
    - CNN
typora-root-url: .
---



# 花里胡哨的卷积

## 普通的卷积（Convolution）

![image-20201121193102270](/../img/QNN/image-20201121193102270.png)

### 基本信息

*  $H_0$ = (H+2* padding -K +1)/stride
*  $W_0$ = (W +2*padding -K +1)/stride
*  ![image-20201121194534204](/../img/QNN/image-20201121194534204.png)
*  kernel_size ：卷积核的大小，即图上的K*K
*  padding： 边缘是否需要填充，填充的大小

## Pointwise Convolution

![image-20201121194921499](/../img/QNN/image-20201121194921499.png)

### 基本信息

* 是个一维的卷积，卷积核大小 1*1 

### example

![image-20201121195118941](/../img/QNN/image-20201121195118941.png)

## Group Convolution

![image-20201121195500028](/../img/QNN/image-20201121195500028.png)

### 基本信息

* 将channel分为N个部分，对着N个部分分别卷积
* ![image-20201121195554650](/../img/QNN/image-20201121195554650.png)

* 基础作用： 一块GPU放不下， 可以将N个部分，放到多个GPU上训练。

### Example

  Depth-wise/Channel-wise Convolution: 

* 每个通道分一个组（分为$C_i$个group）
* ![image-20201121200106811](/../img/QNN/image-20201121200106811.png)

* ![image-20201121200131833](/../img/QNN/image-20201121200131833.png)

## 空洞卷积（Dilated Convolution）



![image-20201121200525962](/../img/QNN/image-20201121200525962.png)



### 基本信息

* ![image-20201121200555112](/../img/QNN/image-20201121200555112.png)
* dilatation： 中间间隔几个空洞，上图中中间那个，每隔一个空洞一个，dilation=1
* 空洞用“0”填充



## 反卷积/转置卷积（Transpose Convolution）

![image-20201121201151617](/../img/QNN/image-20201121201151617.png)

### 基本信息

* 正常的卷积，会将图变小或者大小不变（通过padding）
* 反卷积：上采样，通过卷积将图片变大



----

# 常见的CNN

##  LeNet-5

![image-20201121202142572](/../img/QNN/image-20201121202142572.png)

> 注： Pytorch 搭建网络，需要先对网络初始化（见图__init__(self)方法），然后再搭建网络



## AlexNet

![image-20201121202630142](/../img/QNN/image-20201121202630142.png)

### 基本信息

* 多卡训练(intra-GPU), 最后拼接
* 卡(GPU)内操作：feature map到下一个feature map
* 卡外操作： 全连接等操作

> 注：Feature Map(特征图)是输入图像经过神经网络卷积产生的结果

## ZFNet

![image-20201121203141110](/../img/QNN/image-20201121203141110.png)

## VGGNet

![image-20201121203159694](/../img/QNN/image-20201121203159694.png)

### 基本信息

* 用更多的小卷积核取代大卷积
* 设置卷积block（e.g., conv+conv+conv+Pool ），相当于搞成一个**组件**，减少重复调用的次数。

## Inception

![image-20201121204413960](/../img/QNN/image-20201121204413960.png)

![image-20201121204609476](/../img/QNN/image-20201121204609476.png)

## ResNet

![image-20201121204839970](/../img/QNN/image-20201121204839970.png)

## Others

* DenseNet
* SENet: 认为每个channel的重要性不同，

![image-20201121205504451](/../img/QNN/image-20201121205504451.png)