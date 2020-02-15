---
！1layout:     post
title:      "Deep Learning Compiler"
subtitle:   " \"tvm介绍\""
date:       2019-12-01 13：43
author:     "Jack-C"
header-img: "img/post-bg-DL.jpg"
catalog: true
tags:
    - DL
    - Compiler
    - tvm
---

## 1、TVM解决的用户痛点

- 深度学习框架：方便使用，但是效率低下 &&硬件后端性能发挥不出来
- TVM：深度学习框架和 硬件后端的**桥梁**，，使得深度学习model能够**高效**地部署到各个硬件后端

## 2、TVM主要功能

- 将Keras 、caffe、TF、MXnet各深度学习model**<u>编译</u>**成最小的可部署模块。
- Infrastructure to automatic generate and optimize tensor operators on more backend with better performance.（在更多后端以最佳性能**自动生成并优化**tensor operators的基础架构）



