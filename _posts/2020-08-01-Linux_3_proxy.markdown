---
layout:     post
title:      "Linux运维"
subtitle:   " \"服务器配置外网\""
date:       2020-08-01 15：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Linux
    - 运维
---

## 一、服务器配置代理

1. 确定代理服务器的IP：端口号
2.  在/etc/profile中添加一下代码

```bash
export http_proxy=http://***.**.**.**:端口号
export https_proxy=http://***.**.**.**:端口号
```



## 二、yum配置代理

1. 进入 **/etc/yum.conf** 文件
2. 添加代理如下（代理和/etc/profile代理一致）
3. proxy=http://10.10.44.251:6588     #具体代理见、etc/profile文件



## 三、 maven配置代理

- 在setting.xml文件中配置proxy，详见 

[]: https://jikechao.github.io/2020/02/15/Linux_2_mvn/	"maven踩坑"







------

注：仅配置http协议，所以git clone git@*****   使用的是git协议，无法使用*