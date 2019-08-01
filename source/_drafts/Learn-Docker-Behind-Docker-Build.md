---
title: Docker 学习笔记之 docker build 背后
date: 2019-04-22 23:20:14
tags: 
  - Docker
categories:
  - 学习
---

今天面试 CSIG 被问到 `docker build` 的原理。感觉自己答的不是面试官想要的结果。于是特此整理一下。

以下是我的原回答：

`docker build` 就是通过 `Dockerfile` 编译出一个自定义的镜像（image）。具体过程是：`Dockerfile` 里面的一行一行的命令一层层地生成镜像。如果某一行被改了，那么如果再 build 一次的话就会从这一行开始 build 新的镜像，而之前上一行所 build 的镜像并不会改变。

然后我就被打断了。面试官问：

其实我问的不是 `Dockerfile` 内部的语句是怎么运行的。我问的是 `docker build` 这背后发生了什么？

然后我就懵圈了，我又简单复述了一遍刚刚说所的，然后就果断说就了解这些。（真的不会了。。。）

面试官一听就感觉我不太行就换了其他的问题，比如 LCU

## What is `Docker build` ?

> Build an image from a Dockerfile

## 参考

1. [Docker 镜像之进阶篇](https://www.cnblogs.com/sparkdev/p/9092082.html)
2. 