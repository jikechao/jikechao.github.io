---
layout:     post
title:      "搞懂Python多进程"
subtitle:   " \"Python Multiprocessing使用介绍\""
date:       2020-11-21 19：26
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Python
    - 并发
typora-root-url: .
---

## Abstract

>为了更好地利用服务器的多核资源，需要使用mutiprocessing包，不然程序只在单核运行，效率大大降低。

## 基本用法1

* 将需要运行的任务，封装成一个函数（job）
* 定义进程，传入参数，执行

```python
import mutiprocessing as mp 

def job(name)
	print(name)

p1 = mp.Process(target=job, args=(1,))  # args 要传入一个可迭代的元素（比如数组），要只传入一个数字，使用（数字，）
p2 = mp.Process(target=job, args=(2,))

p1.start()  # 执行子进程p1, 
p2.start()  # 执行子进程p2；注：不需要等到p1执行完，p1,p2并行运行。

p1.join()  # 在此等候p1 执行结束，再运行下一行
p2.join() 

print("finish all subprocesses")  # 当p1,p2都执行完了，才会输出；  注：当删除p1.join()和p2.join()，此行会不需要等待p1,p2执行完成，立即执行。



```





## 使用Pool【进程池】，创建很多个进程

* 创建一个进程池，声明进程的数量（最好不要超过CPU核心数）
* 声明进程创建函数，系统会自动分配/回收进程。

```python
import mutiprocessing as mp 

def job(a)
	print(a)
	
if __name__ == '__main__':
    pool = mp.Pool(processes=8)
    res = pool.map(job,rang(10))
    
    pool.close()
    pool.join()

    for r in res:  # res 为函数返回结果集合
        print(r)
```





## 创建4个进程，执行12个任务

> 此方法和进程池Pool功能相同，但是使用进程池可能会遇到一些错误，可以尝试使用此方法作为替代方法

```python
from multiprocessing import Process, Manager
import time
import itertools

def do_work(in_queue, out_list):
    while True:
        item = in_queue.get()

        # exit signal
        if item == None:
            return

        # fake work
        import numpy as np
        work_time = np.random.random()

        time.sleep(5)
        result = item
        print(f"finish task: {item}!during time:{work_time}")
        print(in_queue.qsize())

        out_list.append(result)


if __name__ == "__main__":
    num_workers = 4

    manager = Manager()
    results = manager.list()
    work = manager.Queue(num_workers)

    # start for workers
    pool = []
    for i in range(num_workers):
        p = Process(target=do_work, args=(work, results))  # 先创建进程，但是work中没有数据，直到work四个数据了，才会执行。
        p.start()
        pool.append(p)
    print('........')

    iters = itertools.chain([1,2,3,4,5,6,7,8,9,10,11,12], (None,)*num_workers)
    for item in iters:
        work.put(item)
        # 当work中有num_workers个任务的时候，触发子进程执行任务；
        # put()不是一次性，全部输入进去，而是再work中的任务数量不够num_workers个的时候，执行put,执行完了就停止等待。
        print(f"execute task: {item}")
    print('.....................')

    for p in pool:  # wait until all child processes terminates
        p.join()


    print(results)
```

