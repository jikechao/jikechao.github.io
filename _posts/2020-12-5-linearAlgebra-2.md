---
layout:     post
title:      "LinearAlgebra 2"
subtitle:   " \"大白话线性空间， 线性空间\""
date:       2020-12-8 19：26
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - LinearAlgebra
ypora-root-url: .
---

## 线性运算

> 定义：同时满足“加法公理”和“数乘公理”的就是线性运算。

* 加法公理
  * a+b=b+a
  * a+(b+c) = (a+b)+c
  * a+0=a
  * a+(-a)=0
* 数乘公理
  * $\lambda(\mu a) =(\lambda \mu)a$
  * $\lambda(a+b)= \lambda a + \lambda b$
  * $(\lambda + \mu)a =\lambda a + \mu a$
  * 1$*$a=a

## 线性空间

> 定义: 对**线性运算**封闭的非空**集合**

线性空间的理解:

* 是非空集合
* 集合中的元素满足线性运算
* 对加法和数乘运算**封闭**

举例:

证明:  连续函数C[a,b] 是线性空间

* 连续函数+连续函数=连续函数
* $(\lambda +\mu)* 连续函数=\lambda* 连续函数+ \mu* 连续函数   $
* ....  满足线性运算
* 运算结果还是连续函数, 运算封闭

常见的线性空间

* R、C
* $R^n$
* $R^{n*n}$
* C[a,b]

* $l^\infty$: **有界**数列空间:   全体实(复)的**有界**数列的集合.
* $l^2 $:   p方可和数列 :  ${(\xi_1,\xi_2,\xi_3,..)|   \space\space \sum_{i=1}^\infty|\xi_i|^2 < +\infty }$



## 线性空间的子空间

> 线性空间X的子集Y,也满足封闭性质, 则这个子集Y就是一个线性空间,称Y是X的空间.



## 注意事项:

1. 子空间的并集不一定是子空间
2. 子空间的交集一定是子空间.
3. $M_1 + M_2 ={x_1+x_2 | x_1 \in M_1, x_2 \in M_2}$ , 是子空间.

## 生成子空间

> M 是包含X的子集, span M 是X的包含M的最小**子空间**



## 基与维数

> X是K上的线性空间, B $\subset X $, 若B线性无关,且span B =X, 则B是X的一个基.  若B是有限集合,则B为有限基.
>
> 若线性空间具有一个有限维基,则X是有限维的;   若X具有一个无限维的基, 则X是无限维的.



X 的维数dim X = 基包含的元素个数.   规定dim{0}=0.

例子:

* dim $R^n =n$
* dim $C^n = n$
* dim $P_n[a,b] = n+1$   # 基为$[1,x,x^2, ..., x^n]$  , 次数不超过n的多项式
* dim P[a,b] = + $\infty$
* dim $R^{m*n}$ = m*n



## 线性算子

1. 定义:  X,Y都是线性空间, 若映射T:  X-->Y 满足, 对于任意的$x_1, x_2, \lambda$,  都满足下面的等式,  则称T是线性的,   这种线性的**映射**叫线性算子.

$$
T(x_1 + x_2) = Tx_1 + Tx_2 
$$

$$
T(\lambda x) = \lambda Tx
$$







#### 映射是线性算子的充要条件:

​    **线性空间**X,Y,  T是X到Y的映射,  对于**任意**的x,y $\subset X, \forall \lambda, \mu \in K$, 皆满足$T(\lambda x + \mu y ) = \lambda T(x) + \mu T(y)$



## 线性算子的运算

对于$\forall T,S 都是将X映射到Y的线性算子  $

* 加法运算  (T+S)(x) = Tx + Sx
* 数乘运算 $(\lambda T)(x) = \lambda Tx$





## 总结

![线性空间](D:\blog\jikechao.github.io\img\2020-12-5-linearAlgebra-2\线性空间.jpg)

