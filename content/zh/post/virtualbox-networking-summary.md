---
title: "VirtualBox 网络总结"
date: 2019-08-23T19:38:55+08:00
draft: true
---

最近想在本地跑一下 k8s，因此想用 `VirtualBox` 虚拟几个节点来模拟一下。一装 CentOS，没有网络，才想起来虚拟机还有好多种网络模式，趁此机会总结一下 VirtualBox 的几个网络模式，包括 `Host-only`、`Internal`(内部网络)、`Bridged Networking`（桥接网络）、`NAT`、`NATservice`。

## VirtualBox 的网络设置

看了下官方手册 [UserManual](https://www.virtualbox.org/manual/UserManual.html#settings-network) 的[网络部分](https://www.virtualbox.org/manual/UserManual.html#nichardware)，才知道虚拟网卡不仅仅有上面的 5 种网络模式，而是多达 8 种，但我们还是主要讨论下这 5 种最重要的吧（~~我怕我写不下去了~~）。而且每台虚拟机能配置多个网卡，之前傻傻的我以为只能一台一个，就像一夫一妻制度。



对于每张虚拟网卡，你都可以从两方面来选：

1. 虚拟的硬件类型。
2. 虚拟网卡的网络模式。



首先，放张表你们感受下：

| **Mode**   | **VM→Host** | **VM←Host** | **VM1↔VM2** | **VM→Net/LAN** | **VM←Net/LAN** |
| ---------- | ----------- | ----------- | ----------- | -------------- | -------------- |
| Host-only  | **+**       | **+**       | **+**       | –              | –              |
| Internal   | –           | –           | **+**       | –              | –              |
| Bridged    | **+**       | **+**       | **+**       | **+**          | **+**          |
| NAT        | **+**       | 端口转发    | –           | **+**          | 端口转发       |
| NATservice | **+**       | 端口转发    | **+**       | **+**          | 端口转发       |

注：上图的 VM 代表虚拟机，Host 代表本机，+ 代表能通，- 通不了。来源 [**Table 6.1. Overview of Networking Modes**](https://www.virtualbox.org/manual/UserManual.html#networkingmodes)



很容易你就会发现 `Bridged` （桥接）这个模式有点牛啊，什么都支持，真的是这样的吗？让我们慢慢道来。



