---
layout:     post
title:      "Python基础用法"
subtitle:   " \"python decorator 注解\""
date:       2020-08-30 19：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - python
---

> python 装饰器（类似Java注解）的基本用法，本质就是**函数内嵌套函数**，起到增强函数作用。

鸣谢：[知乎大佬][1]



### 1、需求

装饰器最大的优势是用于解决重复性的操作，其主要使用的场景有如下几个：

* 计算函数运行时间

- 给函数打日志
- 类型检查



### 2、不带参数装饰器

##### 代码示例

~~~python
import time
def runtime(func):   # 显示程序运行时间的装饰器
    def wrapper():
        begin = time.time()
        func()  # 函数在此运行
        end = time.time()
        print("run time {}".format(end - begin))
    return wrapper
##########################
@runtime
def my_fun():  #主程序
    time.sleep(1)

~~~

##### 代码解读

将主程序my_fun()函数作为参数，通过@runtime，传入到runtime函数的参数func中。

runtime函数，return时，调用wrapper函数

wrapper（）函数，将主程序作为其一部分运行。起到**热插拔**作用

通俗来讲：将主函数外面**包裹**一层。（装饰-主函数-装饰）

### 3、带参数的装饰器

##### 代码示例

~~~python
import time
def logger(msg=None):
    def runtime(func):
        def wrapper():
            begin = time.time()
            func()  # 函数在此运行
            end = time.time()
            print("[{}]run time {}".format(msg, end - begin))
        return wrapper
    return runtime   # 别忘了return

########################################
@logger(msg="level-1")
def my_fun():
    time.sleep(1)

my_fun()
~~~

##### 代码解读

在外面又裹了一层，用来接收参数呗。



### 参考文献

[1]: https://www.zhihu.com/question/325817179/answer/798679602	"通俗地讲解Python的装饰器？"

