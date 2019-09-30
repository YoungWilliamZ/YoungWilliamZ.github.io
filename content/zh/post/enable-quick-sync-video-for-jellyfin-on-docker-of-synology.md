---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "群晖系统中为 Jellyfin 开启硬件加速(Quick Sync Video)"
subtitle: ""
summary: "把踩的坑总结一下，记录下来"
authors: ["admin"]
tags: ["群晖", "docker", "Jellyfin", "硬件加速"]
categories: ["搞机"]
date: 2019-10-01T03:16:51+08:00
lastmod: 2019-10-01T03:16:51+08:00
featured: false
draft: false

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

最近折腾群晖，发现 `Jellyfin` 这个玩具，可以解决用网页看视频的需求。

折腾的途中发现有硬件加速的功能，然后发现确实有些编码格式的视频播放的时候CPU占用很高。

看了一些[评测](http://tech.feng.com/2015-12-23/Synology_DiskStation_DS716+_FengLab_2.shtml)之后发现硬件加速可以很大程度上提升视频解码的能力，同时CPU占用不会很高。

然后发现我的 U，`J1900` 在 [Intel 官网的介绍](https://ark.intel.com/content/www/us/en/ark/products/78867/intel-celeron-processor-j1900-2m-cache-up-to-2-42-ghz.html) 中是支持 `Quick Sync Video`。

于是开始了新一轮的折腾之路。

## 过程

查阅一些资料之后发现，起硬件加速作用的主要是 `/dev/dri`。

在映射 `/dev/dri` 到 docker 之后，选择 Jellyfin 硬件加速的 `Intel Quick Sync Video` 选项发现CPU的占用率几乎没有改变。这是怎么回事？

正百思不得其解时，突然在[论坛](https://www.reddit.com/r/jellyfin/comments/bgbkye/trancoding_in_docker_using_qnap_nas_with_intel/)中发现 `Intel Quick Sync Video` 选项只对 `Windows` 的版本有用。 `Linux` 都是统一用 `VA API` 这个选项。

## 结果

在使用硬件加速之后，确实效果有提升，而且CPU占用是肉眼可见的降低了，由原来的 99% 降低到 50%。播放也不卡顿了。

## 参考

1. [Enabling Quick Sync Video on Handbrake for Docker on Synology](https://wildestpixel.co.uk/enabling-quick-sync-video-on-handbrake-for-docker-on-synology/)
2. https://www.reddit.com/r/jellyfin/comments/bgbkye/trancoding_in_docker_using_qnap_nas_with_intel/
3. https://ark.intel.com/content/www/us/en/ark/products/78867/intel-celeron-processor-j1900-2m-cache-up-to-2-42-ghz.html)
