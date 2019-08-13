---
title: Docker 背后： Linux 的 /proc
date: 2019-08-02 02:04:09
tags:
  - linux
  - docker
  - proc
categories:
  - 学习

authors: ["admin"]
reading_time: true  # Show estimated reading time?
share: false  # Show social sharing links?
profile: false  # Show author profile?
comments: true  # Show comments?
summary: "我的 Docker 原理探索之路"
---

## 为什么要有 `/proc`？

想必在日常开发~~写~~查 BUG 中，你会经常用到像 `top`、`ps` 这样的 Linux 命令来查看进程、CPU 的状态吧。

那你是否有想过：这些不都是内核才知道的吗？而作为身处 shell （用户态）的我们又是通过什么途径知道这些的呢？

是有什么 system call 吗？是有什么高级的接口吗？

不不不，其实是通过一个神奇的目录，也就是今天的主角 `/proc`。

实际上，`top` 中的数据就是读取 `/proc/stat` 文件获得的，`/proc/stat` 文件被读取时会自动更新，从而 `top` 获得最新的 CPU 占用率情况。



## `/proc` 是什么？

`/proc` 其实不是一个“真正”的文件目录，而是一个“虚拟”的文件系统（virtual filesystem）。

为什么是虚拟的呢？因为它不写入**外存**空间，只存在于**内存**中。

它包含了系统运行时的信息，包括：

* 当前运行进程信息
* 系统内存
* mount 设备信息
* 一些硬件配置
* ...等。

因此，可以把它当作**内核**的信息中心。

当然许多系统工具都是通过简单读取该目录下的某些内容。

比如：

1. `lsmod` = `cat /proc/modules`
2. `lspci` = `cat /proc/pci`

在系统正在运行时，你甚至可以通过修改 `/proc` 目录下文件，来读取修改系统的内核参数（sysctl）。



## `/proc` 基本目录结构

因为文件都在内存中，如果你去看每个文件的大小，你会发现都是 0，除了 `kcore`, `mtrr` and `self`。

当遍历这个目录的时候，会发现有些数字，这些都是为每个进程创建的空间，数字就是它们的 PID。

```shell
[go_dev@0f8b372ed635 proc]$ ll /proc
total 0
dr-xr-xr-x  9 go_dev go_dev     0 Aug  1 18:43 1
dr-xr-xr-x  9 go_dev go_dev     0 Aug  1 18:44 30
drwxrwxrwt  2 root   root      40 Aug  1 18:43 acpi
-r--r--r--  1 root   root       0 Aug  1 18:44 buddyinfo
dr-xr-xr-x  4 root   root       0 Aug  1 18:43 bus
-r--r--r--  1 root   root       0 Aug  1 18:44 cgroups
-r--r--r--  1 root   root       0 Aug  1 18:44 cmdline
-r--r--r--  1 root   root   23709 Aug  1 18:44 config.gz
-r--r--r--  1 root   root       0 Aug  1 18:44 consoles
-r--r--r--  1 root   root       0 Aug  1 18:44 cpuinfo
-r--r--r--  1 root   root       0 Aug  1 18:44 crypto
-r--r--r--  1 root   root       0 Aug  1 18:44 devices
-r--r--r--  1 root   root       0 Aug  1 18:44 diskstats
-r--r--r--  1 root   root       0 Aug  1 18:44 dma
dr-xr-xr-x  2 root   root       0 Aug  1 18:44 driver
-r--r--r--  1 root   root       0 Aug  1 18:44 execdomains
-r--r--r--  1 root   root       0 Aug  1 18:44 fb
-r--r--r--  1 root   root       0 Aug  1 18:44 filesystems
dr-xr-xr-x  8 root   root       0 Aug  1 18:43 fs
-r--r--r--  1 root   root       0 Aug  1 18:44 interrupts
-r--r--r--  1 root   root       0 Aug  1 18:44 iomem
-r--r--r--  1 root   root       0 Aug  1 18:44 ioports
dr-xr-xr-x 31 root   root       0 Aug  1 18:43 irq
-r--r--r--  1 root   root       0 Aug  1 18:44 kallsyms
crw-rw-rw-  1 root   root    1, 3 Aug  1 18:43 kcore
-r--r--r--  1 root   root       0 Aug  1 18:44 key-users
crw-rw-rw-  1 root   root    1, 3 Aug  1 18:43 keys
-r--------  1 root   root       0 Aug  1 18:44 kmsg
-r--------  1 root   root       0 Aug  1 18:44 kpagecgroup
-r--------  1 root   root       0 Aug  1 18:44 kpagecount
-r--------  1 root   root       0 Aug  1 18:44 kpageflags
-r--r--r--  1 root   root       0 Aug  1 18:44 loadavg
-r--r--r--  1 root   root       0 Aug  1 18:44 locks
-r--r--r--  1 root   root       0 Aug  1 18:44 meminfo
-r--r--r--  1 root   root       0 Aug  1 18:44 misc
-r--r--r--  1 root   root       0 Aug  1 18:44 modules
lrwxrwxrwx  1 root   root      11 Aug  1 18:44 mounts -> self/mounts
dr-xr-xr-x  2 root   root       0 Aug  1 18:44 mpt
-rw-r--r--  1 root   root       0 Aug  1 18:44 mtrr
lrwxrwxrwx  1 root   root       8 Aug  1 18:44 net -> self/net
-r--r--r--  1 root   root       0 Aug  1 18:44 pagetypeinfo
-r--r--r--  1 root   root       0 Aug  1 18:44 partitions
crw-rw-rw-  1 root   root    1, 3 Aug  1 18:43 sched_debug
lrwxrwxrwx  1 root   root       0 Aug  1 18:43 self -> 30
-rw-------  1 root   root       0 Aug  1 18:44 slabinfo
-r--r--r--  1 root   root       0 Aug  1 18:44 softirqs
-r--r--r--  1 root   root       0 Aug  1 18:44 stat
-r--r--r--  1 root   root       0 Aug  1 18:44 swaps
dr-xr-xr-x  1 root   root       0 Aug  1 18:43 sys
--w-------  1 root   root       0 Aug  1 18:43 sysrq-trigger
dr-xr-xr-x  2 root   root       0 Aug  1 18:44 sysvipc
lrwxrwxrwx  1 root   root       0 Aug  1 18:43 thread-self -> 30/task/30
crw-rw-rw-  1 root   root    1, 3 Aug  1 18:43 timer_list
dr-xr-xr-x  4 root   root       0 Aug  1 18:44 tty
-r--r--r--  1 root   root       0 Aug  1 18:44 uptime
-r--r--r--  1 root   root       0 Aug  1 18:44 version
-r--------  1 root   root       0 Aug  1 18:44 vmallocinfo
-r--r--r--  1 root   root       0 Aug  1 18:44 vmstat
-r--r--r--  1 root   root       0 Aug  1 18:44 zoneinfo
```

在此介绍几个比较重要的部分：

| /proc/N         | PID 为 N 的进程信息                    |
| --------------- | -------------------------------------- |
| /proc/N/cmdline | 进程启动命令                           |
| /proc/N/cwd     | 链接到进程当前工作目录                 |
| /proc/N/environ | 进程环境变量列表                       |
| /proc/N/exe     | 链接到进程的执行命令文件               |
| /proc/N/fd      | 包含进程相关的所有文件描述符           |
| /proc/N/maps    | 与进程相关的内存映射信息               |
| /proc/N/mem     | 指代进程持有的内存，不可读             |
| /proc/N/root    | 链接到进程的根目录                     |
| /proc/N/stat    | 进程的状态                             |
| /proc/N/statm   | 进程使用的内存状态                     |
| /proc/N/status  | 进程状态信息，比 stat/statm 更具可读性 |
| /proc/self/     | 链接到当前正在运行的进程               |

如果像了解更多的话，请自行 google 或者看看 [Linux Filesystem Hierarchy: 1.14. /proc](https://www.tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html)。



## Docker 如何利用到 `/proc`？



## 参考

1. [Linux Filesystem Hierarchy: 1.14. /proc](https://www.tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html)
2. [《自己动手写Docker》](https://book.douban.com/subject/27082348/)

3. [wiki: procfs](https://en.wikipedia.org/wiki/Procfs)