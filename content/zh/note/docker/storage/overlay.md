---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Overlay 文件系统"
linktitle: "Overlay"
summary:
date: 2019-09-14T16:33:30+08:00
lastmod: 2019-09-14T16:33:30+08:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - Substitute `example` with the name of your course/documentation folder.
# - name: Declare this menu item as a parent with ID `name`.
# - parent: Reference a parent ID if this page is a child.
# - weight: Position of link in menu.
menu:
  docker:
    name: Overlay
    parent: Storage 存储相关
    weight: 1
---

## 前言

最近需要把之前做的 [openCredit](https://github.com/YoungWilliamZ/openCredit) 项目重新部署一下，发现好久不用的 `aliyun` 云服务器的磁盘已经快满了。

使用 `df -h` 命令一看：

``` bash
➜  ~ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        40G   37G  820M  98% /
devtmpfs        910M     0  910M   0% /dev
tmpfs           920M     0  920M   0% /dev/shm
tmpfs           920M  716K  919M   1% /run
tmpfs           920M     0  920M   0% /sys/fs/cgroup
overlay          40G   37G  820M  98% /var/lib/docker/overlay2/55aedbcf20a0380e7e03075a7a97a59869ba9c61848a28c3eb8dacc6df00e01d/merged
overlay          40G   37G  820M  98% /var/lib/docker/overlay2/7bc1cd18dea024f05c9f4db97a2d144ffb72da89bbe2e20d1804766f38689900/merged
overlay          40G   37G  820M  98% /var/lib/docker/overlay2/095e388787b7a9c59e79531e509f9dd803d61e328c650836affa7dd110d4052f/merged
shm              64M     0   64M   0% /var/lib/docker/containers/8fd937ba70f9864088e0f6a88a791ff2d1768a38c0acc68a7a51e9ca9b5dbd86/mounts/shm
shm              64M     0   64M   0% /var/lib/docker/containers/33dfef9c8bd1382aad26944ba6dcd946cbb9457588fa1fb81c14bd3261a024f2/mounts/shm
shm              64M     0   64M   0% /var/lib/docker/containers/937a5222d4a6143655844d881a3a2d7f4b42bf50c312c257b43e31d5e024e14c/mounts/shm
tmpfs           184M     0  184M   0% /run/user/0
```

基本都是被 Docker 占用了，37G/40G。好家伙，这个 `overlay` 文件系统竟然这么大，看我怎么治你。

## Overlay 是什么

> OverlayFS is a modern union filesystem that is similar to AUFS, but faster and with a simpler implementation.

`OverlayFS` 是一个现代化的联合挂载文件系统，跟 `AUFS` 很像。

什么是联合挂载呢？

它能够将两个文件目录融合成一个，既：两个目录经过联合挂载之后，用户看到只有一个目录。

`OverlayFS` 于2014年被合并到Linux内核的3.18版本。其4.0版本带来了必要的改进，例如Docker中所需的overlay2存储驱动程序。

OverlayFS呈现其中一个所产生的对象（如果有），“上层”文件系统优先。OverlayFS与其他覆盖型文件系统不同，OverlayFS合并的目录子树不一定是来自不同的文件系统。

OverlayFS支持在上层文件系统中的 `whiteout` 和 `opaque` 目录，以允许删除文件和目录。

## Overlay 在 Docker 中的作用

Overlay 文件系统是 Docker 中的一种 `storage driver`，同时 `overlay2` 也是官方推荐的联合挂载文件系统。

### 什么是 `Storage drivers`

> Storage drivers allow you to create data in the writable layer of your container. The files won’t be persisted after the container is deleted, and both read and write speeds are lower than native file system performance.

`Storage drivers`，或者说 `存储驱动`（我自己翻译的，不知道对不对哈），允许在容器中创建一层可写的数据层。容器一旦被删除，这一层可写数据层也被删除，而且这层数据层的读写速度要比原生的的文件系统要慢。

除了 `overlay2` 之外，Docker 还支持其他的 `storage drivers`:

* `aufs`(Another Union File System)，在老系统不支持 `overlay2` 时，`aufs` 是官方推荐的，包括Docker 18.06及以下的版本。
* `devicemapper`
* `btrfs` and `zfs`
* `vfs`

## Overlay 与 Overlay2 的区别

> If you use OverlayFS, use the overlay2 driver rather than the overlay driver, because it is more efficient in terms of inode utilization.

使用 `OverlayFS` 最好使用 `overlay2`， 因为它对 `inode` 的使用更加有效。（这里的 `inode` 是某些文件系统一种记录文件的方式，可以把它当作一个数据结构，具体可以自己查阅相关资料。）

## Overlay 如何工作的

> OverlayFS layers two directories on a single Linux host and presents them as a single directory. These directories are called layers and the unification process is referred to as a union mount. OverlayFS refers to the lower directory as lowerdir and the upper directory a upperdir. The unified view is exposed through its own directory called merged.

OverlayFS 可以将两个目录「层化」（layer，注意这里是动词），并对外展示为一个单独的目录。这些被「层化」的目录叫做「层」（layers，注意这里是名词），这个统一目录的过程就叫做「union mount」（联合挂载）。



## Overlay2 如何工作的



## 杂谈：Overlay 的缔造者

## 参考

1.[Use the OverlayFS storage driver](https://docs.docker.com/storage/storagedriver/overlayfs-driver/)
2.[维基百科：OverlayFS](https://zh.wikipedia.org/wiki/OverlayFS)
