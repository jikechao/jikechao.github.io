---
layout:     post
title:      "pycallgraph 安装"
subtitle:   " \"代码调用可视化\""
date:       2020-09-08 15：17
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - install
    - tools
---

> graphviz软件 + pycallgraph库

## Windows 安装

### 1、安装graphviz软件 

1. 下载地址： https://www2.graphviz.org/Packages/stable/windows/10/cmake/Release/x64/

2. 一路next完成。配置/bin路径到环境变量中

3. cmd 运行dot -v   

   ~~~bash
   dot - graphviz version 2.44.1 (20200629.0846)
   There is no layout engine support for "dot"
   Perhaps "dot -c" needs to be run (with installer's privileges) to register the plugins?
   ~~~

4. 运行dot -c    // 大概需要安装点什么东西

### 2、安装pycallgraph库

~~~bash
pip install pycallgraph
~~~

## Ubuntu安装

### 1、安装graphviz软件 

~~~bash
sudo yum install graphviz
~~~

### 2、安装pycallgraph库

~~~python
pip install pycallgraph
~~~



## 编写demo 

~~~python
def b():
    pass

def c():
    pass

def a():
    b()
    c()
    return

def entry():
    a()


if __name__ == '__main__':
    from pycallgraph import PyCallGraph
    from pycallgraph.output import GraphvizOutput
    from pycallgraph import Config

    graphviz = GraphvizOutput()
    graphviz.output_file = 'call.png'
    config = Config(max_depth=6)

    with PyCallGraph(output=graphviz, config=config):
        entry()
~~~

## 生成的调用图

![1599573565725](../_post_assets/1599573565725.png)





## 



