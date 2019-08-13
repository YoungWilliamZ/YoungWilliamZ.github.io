---
title: OpenWRT 18.06 IPv6 开启 Relay，LAN 口获取 IPv6 公网地址
tags:
  - OpenWRT
  - IPv6
categories:
  - 搞机
date: 2019-03-10 09:48:55

authors: ["admin"]
reading_time: true  # Show estimated reading time?
share: false  # Show social sharing links?
profile: false  # Show author profile?
comments: true  # Show comments?
summary: "此方案失败了，但还是可以参考的，说不定你就成功了？"
---

> 此方案已失败，但还是可以参考的，说不定你就成功了？
>

<!-- more -->

宿舍的路由器一直不太好用，今天把之前买的 K2P 重新刷了 OpenWRT 新版系统。`WAN 6` 可以获取到 `IPv6` 的公网地址，可是发现 `LAN` 口只能获取本地的 `IPv6`，用不了，连不上北邮人，每次都得切到 `SUSTC-Wifi` 才能连，实在非常不方便。于是就研究了几小时绕了一大圈才找到可行的解决办法。在此记录下。

# `WAN 6` 设置

协议：`DHCPv6 客户端`
请求 IPv6 地址：`try`（不能选 force）

# `LAN` 设置

最下面 `DHCP 服务器` 中 `IPv6 设置` 下：

1. `路由通告服务`、`DHCPv6 服务`、`NDP 代理`全部选为`混合模式`。这样 `WAN 6` 和 `LAN` 就都可以获得公网 `IPv6`。
2. `DHCPv6 模式` 选择 `无状态的 + 有状态的`

“上面「一般配置-物理设置」，找到「接口」，点击最右边的小三角，弹出下拉菜单。默认这里「交换机 VLAN eth0.2 (lan, wan, wan6)」是没有选上的吧？勾选上这个。为啥选这个呢？因为它有 wan6 哇～”

保存并应用。等一段时间还没有生效的话，就重启。我是重启完立马就有了原生 `IPv6`。

# 主要参考

[OpenWrt IPv6 中继](https://haoyu.love/blog646.html)

# PS

我自己在之前的教程中修改了 `/etc/config/dhcp`，不知道有没有影响。
只是在`config dhcp 'lan'` 的最下面添加了 `option master '1'`。如果上诉方法对你不起作用，可以试试这个或者使用 `NAT`。
