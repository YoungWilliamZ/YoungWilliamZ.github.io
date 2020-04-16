---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "CSAPP 笔记 第三章《程序的机器级表示》Part.1"
subtitle: "《深入理解计算机》学计算机必读书目"
summary: "重点是理解机器级的函数调用"
authors: ["admin"]
tags: ["CSAPP"]
categories: ["学习"]
date: 2019-09-22T21:01:13+08:00
lastmod: 2019-09-22T21:01:13+08:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

本章的 `3.7` 节重点讲解了函数的调用过程在机器级的表示。

在 3.7 节之前，主要讲解的是汇编语句的格式、顺序执行、跳转控制。

## 过程

### 基本概念

不同的编程语言中，过程的形式不同：

* 函数 (function)
* 方法 (method)
* 子例程 (subroutine)
* 处理函数 (handler)等等

但他们都有的共有特性。

要提供对过程的机器级支持，必须要处理许多不同的属性。

为了讨论方便，假设过程 p 调用过程 Q, Q 执行后返回到 P。这些动作包括下面一个或多个机制:

1. **传递控制。**
   在进入过程 Q 的时候，程序计数器必须被设置为 Q 的代码的起始地址，然后在返回时，要把程序计数器设置为 P 中调用 Q 后面那条指令的地址。
2. **传递数据**。
   P 必须能够向 Q 提供一个或多个参数， Q 必须能够向 P 返回一个值。
3. **分配和释放内存**。
   在开始时， Q 可能需要为局部变量分配空间，而在返回前，又必须
   释放这些存储空间。

