---
title: "我抛弃了zsh，拥抱了fish"
date: 2019-05-19T00:37:50+08:00
lastmod: 2019-05-19T00:37:50+08:00
draft: false
keywords: []
description: ""
#tags: ["编程", "摄影"]
categories: ["编程"]
author: "simon"
---

最近花了好几天时间来配置自己的shell。尝试了很多东西都不是很满意，最后决定还是fish shell。

# zsh
zsh有点主要有一下几点：

<!-- more -->
### 丰富而强大的插件
oh-my-zsh（下面简称omz）, prezto, aitigen, antibody, zgen, zplug等各种插件解决方案，丰富到让人选择困难，更别提其各自支持的插件数量。其中最后有名的omz自带的插件就达到上百种之多，一切想要的功能，几乎都能在上面找到。就算一些没有支持的插件，也很容易通过第三方插件的形式加入到omz当中。各种功能，不可谓不丰富。

然后丰富的功能带来的问题是，随着加载的各个插件的增多，zsh的运行也越来越慢。在加载了四五个插件之后，打开一个新的zsh窗口，在我这个2017年的macbook pro当中也需要3s左右时间。更别提每次换行肉眼可见的延迟。

使用zsh就是为了其丰富的插件，然后加载过多的插件势必造成zsh运行的缓慢。包括其余生成可以提过zsh加载速度的方案（zgen、prezto等），一定程度上都存在这个问题。

### 语法兼容bash

zsh是兼容bash的语法的，为bash写的脚本可以很容易运行在zsh上。

### 用户基数大

不得不说使用zsh的人真的很多，网上各种介绍zsh的文章，各种为zsh写的插件。很多服务提供的shell脚本也会提供zsh的一版。

## fish介绍

### 抛弃zsh，切换到fish
zsh固然有各种好处，耐不住太实在太卡。用起来很影响体验。

对于fish很久之前就用过几次，然而都没有长时间使用过。fish相比zsh开箱即用，自带很多常用命令的补全，而且支持语法高亮和根据历史记录自动建议命令行。这种功能也可以在zsh上实在，要装上zsh-autosuggestions和zsh-syntax-highlighting，不过这两个插件都很重量级，装上之后zsh就会卡很多，这也是这次我想要换fish的主要原因。

fish的语法高亮和自动命令补全真的很好用，很多时候都不用一点点把命令打完整，fish会自动根据你的历史记录，给出可能的命令行，相当好用。

### fish配置

既然决定要用fish shell，也需要好好配置一番，使其更合乎自己的使用习惯。由于zsh的经验，决定不再使用第三方插件管理（fish也有和omz相对应的oh-my-fish (omf) ，也有单独的插件管理工具fisher，相当于zsh的antigen)，自己搜索网上的配置文件，参考别人的实践，手写自己的fish配置。

fish的配置已经上传到了guthub, 哈哈 :[dotfiles](https://github.com/simon-ding/dotfiles)

主要配置了常用的函数，一些alias，还有装了fasd和fzf用来跳转和搜索文件。