---
title: MIPS下实现阿克曼函数（Ackermann Function）
categories:
  - 汇编
tags:
  - AssemblyLanguage
  - MIPS
  - 汇编
author: YoungWilliam
date: 2018-05-17 23:37:00
---
### **什么是阿克曼函数？What is *Ackermann Function* ?**


> #### 来自[维基百科](https://zh.wikipedia.org/wiki/%E9%98%BF%E5%85%8B%E6%9B%BC%E5%87%BD%E6%95%B8)
> 阿克曼函数是非原始递归函数的例子；它需要两个自然数作为输入值，输出一个自然数。它的输出值增长速度非常高。

> #### From [Wikipedia](https://en.wikipedia.org/wiki/Ackermann_function)
> In computability theory, the Ackermann function, named after Wilhelm Ackermann, is one of the simplest[1] and earliest-discovered examples of a total computable function that is not primitive recursive. All primitive recursive functions are total and computable, but the Ackermann function illustrates that not all total computable functions are primitive recursive.

> #### **定义**
$A( m, n) = \begin{cases}
n + 1, & \text{if $m$ = 0}
\\
A(m-1, 1), & \text{if $m$ > 0 and $n$ = 0}
\\
A(m-1, A(m, n-1)), & \text{if $m$ > 0 and $n$ > 0}
\end{cases}
$


$$\begin{equation}
\begin{aligned}
a &= b + c \\
  &= d + e + f + g \\
  &= h + i
\end{aligned}
\end{equation}\label{eq2}$$

$$\begin{align}
a &= b + c \label{eq3} \\
x &= yz \label{eq4}\\
l &= m - n \label{eq5}
\end{align}$$
