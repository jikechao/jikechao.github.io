---
layout:     post
title:      "远程Docker与PyCharm"
subtitle:   " \"服务器docker与pycharm联动调试\""
date:       2023-03-15 21：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Docker
typora-root-url: .
---



## 存在的问题：

> 使用远程服务器中docker内部的Python环境，作为PC的interpreter，使得程序在服务器docker内运行，但能在PyCharm上调试。



## 解决方法

### 1. 拉去image并创建容器

```javascript
docker run --runtime=nvidia -itd --name tvm_latest -v /data/shenqingchao/share_host:/share_container -p 8080:22 tvm:latest /bin/bash
```

> 注： 使用的宿主机端口号一定要是已经开放的端口，否则会出现connection timeout问题！

### 2.  进入容器，并安装ssh服务

```shell
docker exec -it tvm_latest /bin/bash

apt install openssh-server 

```



### 3. 找到 /etc/ssh/  目录下的sshd服务配置文件sshd_config，并打开

```shell
vim /etc/ssh/ssh_config

# 去掉PasswordAuthentication yes前面的#号,保存退出

vi /etc/ssh/sshd_config

# 修改以下三条内容，并检测两个截图内容是否正确。
PasswordAuthentication yes
# 如果不配置Subsystem sftp internal-sftp。就会导致ssh可以连接上，而sftp连接不上
Subsystem sftp internal-sftp         
PermitRootLogin yes



# 修改root 密码
passwd root
```



![image-20230315211624087](/../img/2023-03-15-Docker-pycharm/image-20230315211624087.png)

### 4. 开启ssh服务

```shell
/etc/init.d/ssh start

# 当修改ssh/sshd文件，需要重启ssh服务
/etc/init.d/ssh reload
/etc/init.d/ssh restart


# 检查服务是否开启
ps -e | grep sshd
```



### 5. 连接PyCharm

- setting--> interpreter--> ssh interpreter---> existing server configuration 
- 配置的username： root
- 配置的port： 映射的宿主机端口： 8080

