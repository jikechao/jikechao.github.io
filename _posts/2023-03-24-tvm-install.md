---
layout:     post
title:      "TVM安装"
subtitle:   " \"源码安装TVM\""
date:       2023-03-24 14：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - TVM
typora-root-url: .
---



## 1. 安装LLVM：

直接下载pre-release版本，https://prereleases.llvm.org/9.0.0/

## 2. 安装GCC-7

```shell
apt-get install software-properties-common  #保证add-apt-repository命令安装
add-apt-repository ppa:ubuntu-toolchain-r/test  # 加载镜像源
apt-get update  # 加载后必定更新
apt-get install -y gcc-7 g++-7
ln -s /usr/local/gcc-7 gcc
ln -s /usr/local/g++-7 gcc
```





## 3. 安装Cmake-3.26

```shell
wget https://github.com/Kitware/CMake/releases/download/v3.26.1/cmake-3.26.1-linux-x86_64.tar.gz
tar ....
export PATH：....
```





## 4.下载TVM

* --recursive 
* git submodule update
*  ### 保证所有子模块都下载成功



## 5.配置CUDA环境

* which nvcc
* nvidia-smi
* 保证cuda环境是ok的



## 6. 编译安装

1. 修改config.cmake文件
2. mkdir build; cd build
3. cmake ..
4. make -j32

## 配置Python环境

```shell
export TVM_HOME=/opt/tvm
export PYTHONPATH=$TVM_HOME/python:${PYTHONPATH}

# 安装Python依赖包：
pip3 install --user numpy decorator attrs

其他依赖包详见：
https://tvm.apache.org/docs/install/from_source.html
```

