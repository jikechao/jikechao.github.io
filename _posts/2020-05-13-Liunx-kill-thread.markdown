---
layout:     post
title:      "Linux运维"
subtitle:   " \"已删除文件继续占用空间\""
date:       2020年5月13日 19：43
author:     "Jack-C"
header-img: "img/post-bg-DL.jpg"
catalog: true
tags:
    - Linux运维
    - 内存占用
    - kill 进程
---

## 1、痛点分析

1. 一个超大日志文件被删除，但是仍然占用空间   # du 显示文件已删除， df 显示仍存在
2. 没有程序运行，但内存占用（buff/cache)居高不下

## 2、问题分析

1.  被删除的文件被某个进行打开，已加载到内存中，并且文件尚未被关闭
2.  此时删除了文件，但是并不会删除此文件已加载到内存中的内容，该文件缓存内容存放于/proc/PID/中

## 3、解决方案

1. ```bash
    lsof |grep deleted  # 显示已经被删除但是仍然被应用程序占用的文件列表
    # 找到对应的进行，kill 该进行
    kill -9 PID
    
   ```

2.  若找不到对应的进程，考虑 kill 当前用户所有的进程

   ```bash
   ps -u username | grep -v PID | awk '{print$1}'| xargs kill -9
   # 等价命令如下，实践不奏效
   killall -u username
   ```


## 4、拓展知识



*  lsof :   list open file 

*  PID ： 进程的ID号

*  <a href= 'https://www.runoob.com/linux/linux-comm-kill.html'>Kill命令介绍</a>

  ```bash
  -  kill PID 
  -  kill -KILL PID # 强制kill 进程
  -  kill -9  PID # 彻底kill进程
  ```



