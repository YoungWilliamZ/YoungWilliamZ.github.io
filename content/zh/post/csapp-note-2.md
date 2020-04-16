---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "CSAPP 笔记 第二章《信息的表示和处理》"
subtitle: "《深入理解计算机》学计算机必读书目"
summary: "大四了才开始读这本书，是不是晚了？"
authors: ["admin"]
tags: ["CSAPP"]
categories: ["学习"]
date: 2019-09-08T20:56:59+08:00
lastmod: 2019-09-08T20:56:59+08:00
featured: false
draft: true
share: false  # Show social sharing links?
profile: false  # Show author profile?
comments: true  # Show comments?

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

这章的内容非常基础并且重要，但为了节省篇幅，此篇笔记仅记录笔者不熟的或者认为非常重要且易出错的知识点。

## 为什么要用「二进制」

> “二值信号能够很容易地被表示、存储和传输，例如，可以表示为穿孔卡片上有洞或无洞、导线上的高电压或低电压，或者顺时针或逆时针的磁场。对二值信号进行存储和执行计算的电子电路非常简单和可靠，制造商能够在一个单独的硅片上集成数百万甚至数十亿个这样的电路。”

## "little endian(小端)"和 "big endian(大端)"

最低有效字节在最前面的方式，称为小端法 (little endian) 。

最高有效字节在最前面的方式，称为大端法 (big endian) 。

例子：

假设变量 x 的类型为 int ，位于地址 0x100 处，它的十六进制值为Ox01234567。

| 类型 | 0x100 | 0x101 | 0x102 | 0x103 |
| ---- | ----- | ----- | ----- | ----- |
| 大端 | 01    | 23    | 45    | 67    |
| 小端 | 67    | 45    | 23    | 01    |

### 如何分辨一台机器是大端还是小端

以下代码从 `NGINX` 抄来的。

``` C
#include <stdio.h>
int main() {
    int i = 0x11223344;
    char *p;

    p = (char *) &i;
    if (*p == 0x44) {
        printf("Little endian\n");
    }
    else {
        printf("Big endian\n");
    }
    return 0;
}
```

## 有符号数与无符号数



## 参考

1. [判断计算机是大端还是小端](https://blog.csdn.net/lwfcgz/article/details/50476051)
