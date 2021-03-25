---
layout:     post
title:      "Docker中GPU环境的辛酸泪"
subtitle:   " \"kernel version 430.14.0 does not match DSO version 418.67.0\""
date:       2021-03-25 11：26
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Docker
    - GPU
    - CUDA
typora-root-url: .
---

> 满纸荒唐言，一把辛酸泪。都言作者痴，谁解其中味。

### Docker中使用GPU介绍

1. 一般不需会在容器中安装cuda，都是直接pull一个含有cuda的镜像。
2. 需要安装docker和docker-nvidia2， 创建容器的时候使用命令`docker run --runtime=nvidia ...` , 这样就可以通过调用docker-nvidia。 这样就可以直接开用gpu了，就这么简单。

### NVIDIA驱动和cuda版本不匹配问题

![image-20210325115640399](/../img/2021-3-25-docker-gpu/image-20210325115640399.png)

#### 1. 分析：

* kernel version应该是指NVIDIA的驱动版本，而DSO version应该指的是cuda的版本。
* kernel version这个版本好像还改不了，自动追随宿主机的NVIDIA版本，气不气，那就只能更改DSO version。



#### 2. 查看对应文件的版本

* 相关的文件位置位于：/usr/lib/x86_64-linux-gnu  和 usr/local/cuda/compat  这两个目录下了。
* 通过`nvidia-smi`  查看驱动的版本是430.14版本，说明需要将某个地方的（cuda）418版本改成430才对。

####  3. /usr/lib/x86_64-linux-gnu

通过`ls libcuda.so* ` 查看对象的cuda版本，哎呦喂，还真他发现了两个版本。

![image-20210325122120513](/../img/2021-3-25-docker-gpu/image-20210325122120513.png)

顺路也查一下和下文compat中需要的libnvidia吧，哎呦也有两个版本

![image-20210325122203892](/../img/2021-3-25-docker-gpu/image-20210325122203892.png)

#### 4.usr/local/cuda/compat

![image-20210325121058067](/../img/2021-3-25-docker-gpu/image-20210325121058067.png)

通过配置文件/etc/profile发现，配置的路径中包含/usr/local/cuda/compat 这个路径，然而这个路径吓得文件都是418版本的这还得了，需要改成430版本的。但是这个文件不在这里。而恰巧这些文件都在/usr/lib/x86_64-linux-gnu下， 所以直接创建指到对应文件的软连接很nice，或者把他拷贝过来一份再创建软联机，也很nice。

拷贝libcuda.so.430 和libnvidia-ptxjitcompiler.so.430.  然后创建软连接，完美。

----

问题解决！！！