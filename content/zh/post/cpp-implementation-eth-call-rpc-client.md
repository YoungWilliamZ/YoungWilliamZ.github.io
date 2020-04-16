---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "C++ 实现以太坊 Call RPC接口的发送端"
subtitle: ""
summary: ""
authors: ["YoungWilliam"]
tags: ["CPP", "ETH", "RPC"]
categories: []
date: 2020-03-17T16:14:03+08:00
lastmod: 2020-03-17T16:14:03+08:00
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

## 起因

目前做的毕设中，需要实现一个调用 `FISCO-BCOS` 底层的一个RPC端口，看了[官方文档](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/api.html#call)以及[以太坊的文档](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_call)，发现参数中`params`的`data`的格式有一定要求，需要参照 `Solidity` 的[文档](https://solidity.readthedocs.io/en/latest/abi-spec.html)。

## 经过



## 结尾

