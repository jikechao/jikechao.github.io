---
layout:     post
title:      "Conda 安装"
subtitle:   " \"Miniconda 安装及其配置\""
date:       2020-07-28 15：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Linux
    - 运维
    - Python
    - conda
---



## 1. 下载安装包

通过<a>conda.io</a>找到环境所需要的版本，并下载

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

## 2. 通过脚本安装

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

ps: 根据需要，选择合适的配置，

## 3. 配置环境

- 步骤二会为当前用户配置好环境

- 为所有用户配置环境

  - 将.bashrc中的配置复制到/etc/profile下，供全局使用

  -  添加以下信息，关闭bash 中conda一直处于活动状态

  - ```bash
    conda deactivate
    ```

## 4. conda常用命令

1.  **创建env**

   ```bash
   conda create -n xxxx python=3.5  # 创建新的env
   conda remove -n xxxx --all   #删除环境
   ```

2. **选择env**

```bash
conda env list  # 查看已经安装的env
conda activate base  # 激活base环境
conda deactivate   # 关闭当前conda ， 返回到bash
```

2.  **安装包**

   ```bash
   coonda list   # 查看所有安装的包
   conda install ***
   conda uninstall ***
   ```
