---
layout:     post
title:      "LinearAlgebra 1"
subtitle:   " \"求解方阵函数在某处的值\""
date:       2020-12-5 19：26
author:     "Jack-C"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - LinearAlgebra
ypora-root-url: .
---

> 由方阵作为变量的函数叫做方阵函数f(A)，方阵函数在某具体方阵下的值得求解方法，例如，求$e^{At}$在A=[..]时的值。

## 解法一：根据A的Jordan标准型

特殊情况： 当A本身即为Jordan标准型

解法：

1. 确定每个Jordan块
2. 对每个Jordan块，主对角线元素为f($\lambda$), 次对角,依次向外的行为:$$f_\lambda{'}(\lambda t)$$ ,  $\frac{f_\lambda{''}(\lambda t)}{2!}$,   $\frac{f_\lambda{'''}(\lambda t)}{3!}$,.....

## 解法二：将f(A)表示为A的多项式

原理： 

* 当方阵A的谱半径$\rho(A)$< 收敛半径R， 则存在唯一的m-1次多项式T($\lambda$)，使得f(A)=T(A), 且在A处 的谱值相等。
* $e^x=\sum_{k=0}^{\infty} \frac{X^k}{K!}$ 

步骤：

1. 求解最小多项式$\phi$($\lambda$)
2. 设定f(A) = $a_0(t)E+a_1(t)A+a_2(t)A^2+...$
3. 利用f(A)与右边的多项式谱值相等，列出方程求解
4. 带入（2）求出f(A)
5. PS：谱值就是特征值呀呀！！。多项式的特征值==g($\lambda$)， 若有重根，另一重根的谱值为对t的导数



# 初值问题

$$
\begin{cases}
\frac{dx(t)}{dt}=Ax(t)  \\
x(t_0)=C
\end{cases}
$$

此方程存在唯一解$x(t)=e^{A(t-t_0)}C$

