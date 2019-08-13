---
title: Mac OS X 下优化 Terminal，一篇就够了！
tags:
  - Max OS X
categories:
  - 工欲善其事
date: 2018-08-11 13:52:00

authors: ["admin"]
reading_time: true  # Show estimated reading time?
share: false  # Show social sharing links?
profile: false  # Show author profile?
comments: true  # Show comments?
summary: "Mac 大法好"
---

先上最终效果图：

![](https://i.imgur.com/0goJQf8.png)

## 目录

<!-- TOC -->

- [目录](#%08%e7%9b%ae%e5%bd%95)
- [1. 相关工具介绍](#1-%e7%9b%b8%e5%85%b3%e5%b7%a5%e5%85%b7%e4%bb%8b%e7%bb%8d)
- [2. 配置总览](#2-%e9%85%8d%e7%bd%ae%e6%80%bb%e8%a7%88)
- [3. 安装步骤](#3-%e5%ae%89%e8%a3%85%e6%ad%a5%e9%aa%a4)
  - [3.1. 安装 iTerm2](#31-%e5%ae%89%e8%a3%85-iterm2)
  - [3.2. 安装XCode’s Command line tools](#32-%e5%ae%89%e8%a3%85xcodes-command-line-tools)
  - [3.3. 检查 `zsh` 是否已安装](#33-%e6%a3%80%e6%9f%a5-zsh-%e6%98%af%e5%90%a6%e5%b7%b2%e5%ae%89%e8%a3%85)
  - [3.4. 安装 `Oh-My-Zsh`](#34-%e5%ae%89%e8%a3%85-oh-my-zsh)
  - [3.5. 安装 `Powerline fonts`](#35-%e5%ae%89%e8%a3%85-powerline-fonts)
  - [3.6. 安装配置主题](#36-%e5%ae%89%e8%a3%85%e9%85%8d%e7%bd%ae%e4%b8%bb%e9%a2%98)
  - [3.7. 修改命令提示符](#37-%e4%bf%ae%e6%94%b9%e5%91%bd%e4%bb%a4%e6%8f%90%e7%a4%ba%e7%ac%a6)
  - [3.8. 配置 `zsh` 命令语法高亮](#38-%e9%85%8d%e7%bd%ae-zsh-%e5%91%bd%e4%bb%a4%e8%af%ad%e6%b3%95%e9%ab%98%e4%ba%ae)
- [4. 备用下载链接](#4-%e5%a4%87%e7%94%a8%e4%b8%8b%e8%bd%bd%e9%93%be%e6%8e%a5)
- [5. 参考及感谢](#5-%e5%8f%82%e8%80%83%e5%8f%8a%e6%84%9f%e8%b0%a2)

<!-- /TOC -->

<!-- more -->

## 1. 相关工具介绍

1. **iTerm2**： Terminal 终端的替代品，拥有更多强大的功能，想了解更多请戳 [iTerm2 官网](https://www.iterm2.com/)；
2. **XCode’s Command line tools**: 开发环境集成，包含 git、gcc 等重要工具；
3. **zsh**：Linux 的一种 shell 外壳，强大的虚拟终端，和 bash 属于同类产品，OS X 已自带；
4. **Oh-My-Zsh**: 用来管理 zsh 的配置，同时还有很多社区贡献的主题配置以及好用的插件可供使用，了解更多请戳 [Oh-My-Zsh 官网](https://ohmyz.sh/)；

## 2. 配置总览

1. iTerm2
2. Oh-My-Zsh
3. agnoster 主题
4. zsh 命令语法高亮

## 3. 安装步骤

### 3.1. 安装 iTerm2

进入[官网下载页面](https://www.iterm2.com/downloads.html)，点击 `Stable Releases` 下面的 `Download` 即可下载，解压后拖到应用程序中。这是简单的一小步，但是却是优化 `Terminal` 的一大步！

PS: 网络环境不太好的同学不要担心，我已经把安装包传到百度云了，请到文章最后寻找链接下载。

### 3.2. 安装XCode’s Command line tools
XCode 13 个 G ,这里我们只需要 XCode’s Command line tools 来支撑 Git 的使用，所以我们不用费时费力装 XCode 来浪费那13个G。
(从 Yosemite（10.10+）开始，Command Line Tools 可以单独安装。)

**安装方法**: 打开终端，输入
``` bash
xcode-select –install
```
点 `install` ，同意，即可。

### 3.3. 检查 `zsh` 是否已安装

在命令行输入：

``` bash
zsh --version
```

如果显示

``` bash
zsh 5.3 (x86_64-apple-darwin17.0)
```

即zsh的当前版本号，就说明装好了，一般 OS X 自带有的。

如果没装则需要输入：

``` bash
brew install zsh zsh-completions
```

这是用Homebrew装，需要 OS X 上有Homebrew，它的网站：https://brew.sh/

### 3.4. 安装 `Oh-My-Zsh`

可以通过 `curl` 或者 `wget` 来安装

* curl
  
``` bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

* wget

``` bash
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

网络好的话几秒就装好了。

一般安装程序会自动把默认的 shell 切换为 `zsh`, 什么！你不懂 shell 是什么？我也不懂，你去问问度娘？或者直接右上角？其实不懂也正常，也不影响你拥有自己美美的终端。

如果想修改默认 shell 为原来的 bash：
``` bash
chsh -s /bin/bash
```

重启 iTerm2 就好了。

### 3.5. 安装 `Powerline fonts`
华丽丽的主题需要 [Powerline 字符集](https://github.com/powerline/fonts)的支持。
通过 git 安装, 直接执行以下命令行：
``` bash
git clone https://github.com/powerline/fonts.git --depth=1; cd fonts; ./install.sh; cd ..; rm -rf fonts
```

然后到 iterm2 配置，设置字体
1. 打开 **Preferences**，选择 **Profiles**
![](https://ws4.sinaimg.cn/large/006tNbRwgy1fu4hnix30uj30ye0d5wfk.jpg)
2. 选择 **Text**，点击 **Change Font**
![](https://ws4.sinaimg.cn/large/006tNbRwgy1fu4i6ep6kcj30pi0ciaai.jpg)
3. 选择**固定宽度**，选择自己喜欢的字体。**注意**： iTerm2 可以实时看到效果，结尾不是 `for Powerline` 的会乱码。
![](https://ws4.sinaimg.cn/large/006tNbRwgy1fu4i94dy0uj30ht0eqdg3.jpg)
4. 接着修改字体颜色，选择另一项 `Colors`, 点击 `Color Presets`, 选择`Solarized Dark`
![](https://ws1.sinaimg.cn/large/b48b5d8dly1fu4m84ux8uj20ps0ci40g.jpg)
![](https://i.imgur.com/T8jzFpm.png)

### 3.6. 安装配置主题
装好之后就可以换到 agnoster 主题，Oh My Zsh 一般自带有这个主题。

可以看看其它的默认主题：
``` bash
ls ~/.oh-my-zsh/themes
```
需要修改主题只需：
``` bash
vim ~/.zshrc
```
然后把里面 ZSH_THEME 的值改为 ZSH_THEME="agnoster"，保存退出。

修改和保存的过程为：
1. 按下 `i` 开始编辑
2. 通过方向键控制光标的位置
3. 定位到 `ZSH_THEME`
4. 改为 `ZSH_THEME="agnoster"`
5. 按下 `ESC`, 输入 `:wq`, 回车
6. 搞定！

（[点击这里](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes#agnoster)还有各种主题预览，任君翻牌~）

### 3.7. 修改命令提示符 
默认的命令提示符为 user@userdemackbookPro，这样的提示符配合 powerlevel9k 主题太过冗长，因此我选择将该冗长的提示符去掉，在 `~/.zshrc` 配置文件后面追加如下内容：

``` bash
# 注意：DEFAULT_USER 的值必须要是系统用户名才能生效
DEFAULT_USER="user"
```

编辑方法同上。

### 3.8. 配置 `zsh` 命令语法高亮
[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) 插件可以使你终端输入的命令有语法高亮效果，安装方法如下（oh-my-zsh 插件管理的方式安装）： 
1. 复制文件到插件目录
    ``` bash
    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
    ```
2. 修改 `~/.zshrc` 添加插件
    ``` bash
    # 注意：zsh-syntax-highlighting 必须放在最后面（官方推荐）
    plugins=( [plugins...] zsh-syntax-highlighting)
    ```
    ![](https://i.imgur.com/N6db1vG.png)

3. 应用修改
    ``` bash
    source ~/.zshrc
    ```

大功告成~有问题欢迎留言讨论~


## 4. 备用下载链接
 iTerm2 ( 3.2.0 )： 链接: https://pan.baidu.com/s/1Wm7NjtGfA81LpGFYAaEAPQ 密码: ksgv



## 5. 参考及感谢
1. [我的 Mac 终端配置（Mac OSX + iTerm2 + Zsh + Oh-My-Zsh）](https://blog.csdn.net/qianghaohao/article/details/79440961)
2. [iTerm2 + OhMyZsh + agnoster + Powerline + solarized = 漂亮的Mac终端](https://blog.csdn.net/huihut/article/details/61418136)