---
title: "go 语言 gui 库现状 2019"
date: 2019-09-14T00:24:08+08:00
lastmod: 2019-09-14T00:24:08+08:00
draft: false
keywords: []
description: ""
#tags: ["编程", "摄影"]
categories: ["编程"]
author: "simon"
---

最近突然迷上了go写gui程序，不停的寻找各式各样的go gui库。很遗憾的是，几乎所有库都有各式各样的问题。

## andlabs/ui

[andlabs/ui](https://github.com/andlabs/ui) 是我最早接触的一个gui库。用这个库的起因是因为想为公司写一个桌面程序，简化自己的工作。
<!-- more -->

这个库是存在比较久的一个库了，7k+ star，基本组件还算完善。最终使用了这个库写了一个桌面程序，使用下来感觉还是挺简单的，文档虽然不是很多，但按照给的例子抄着写也不算太麻烦。

但是这个库有个问题是，实在是太不成熟了，github主页上说项目处于 mid-alpha 阶段，使用下来经常会遇到一些莫名其妙的崩溃bug。虽然我写的程序也不是那么关键，但多崩溃了在重新打开就是了，的确很恼人。而且这个库也基本属于不更新的状态了。最新的一条commit是在2018年9月，现在已经基本上一年过去没有更新了。

总结：

优点：
    1. 入门简单
    2. 全平台支持
    3. native look

缺点：
    1. 不成熟，总是崩溃
    2. 文档不全
    3. 开发者不活跃

## fyne-io/fyne

这是另一个看起来比较有潜力的库，github地址 [fyne-io/fyne](https://github.com/fyne-io/fyne)

不错的外观，活跃的社区。文档相比 andlabs/ui 齐全多了，项目竟然有自己的网站！！[Fyne](https://fyne.io/) (研究过go 的gui程序就知道这是有多难得了，大部分项目都没有网站，文档更是只有github的几个例子，而且给的例子能够一步跑通就已经谢天谢地了）

基于OpenGL开发，全部是go代码，没有向好多库都要链接c或c++库，而且官网都已经发布1.0版本了，应该已经很稳定了。还有他竟然连iOS和Android都支持，网站主页甚至给出了一个 fyne 写的桌面环境，NB，NB！

然而理想是丰满的，现实是骨感的。这货竟然连个打开文件的组件都没有，一下子使用的欲望就没了，还好开发者已经在着手解决这个问题了： [Open file dialog](https://github.com/fyne-io/fyne/issues/225) 

这个问题能解决，我还是很想试一试这个库的。

## therecipe/qt

这应该是目前go语言里功能最丰富的gui库了，go语言的qt绑定。项目地址 [therecipe/qt](https://github.com/therecipe/qt)

实现了qt的大部分功能，支持最新的qt5，开发者也非常活跃，一直在持续更新这个库，几乎没几天就有一个commit。

qt本身是一套非常成熟的跨平台gui库，所有遇到的gui问题，都应该能通过qt解决。然而qt是一个非常大的项目，学习成本还是挺高的，qt的官方文档非常齐全。但是有个问题就是qt的官方文档是面向c++的，不会c++看起来非常痛苦。而 therecipe/qt 项目几乎没有提供什么文档，官方例子很多，但是配置太过于复杂，我好多都运行不成功。

优点：
    1. 实现qt大部分功能，功能丰富
    2. 开发者活跃，一直有更新
    3. qt生态非常成熟，文档比较全

缺点：
    1. 文档太少，代码注释也没有（都是生成的代码）


总的来说这应该是目前go gui程序最成熟，功能最丰富的方案。然而项目还是比较早期，文档非常欠缺，网上使用的例子也非常少。学习难度比较大。当然你如果懂C++的话难度会较低很多，qt官方文档很丰富。不过话说回来既然懂C++为啥不直接用C++开发的，原生绑定，资料又多。
    
    
## 总结
    
这里只提到了这三个库，还有基于electron的 [go-astilectron](https://github.com/asticode/go-astilectron)，基于sciter的 [go-sciter](https://github.com/sciter-sdk/go-sciter)，基于 nuklear 的 [golang-ui/nuklear](https://github.com/golang-ui/nuklear) 等等，然而过于小众，electron 又太笨重。
    
还有一个值得一提的项目是 [go-flutter-desktop/go-flutter](https://github.com/go-flutter-desktop/go-flutter)，用flutter来写写桌面程序，这是目前大热的flutter的go语言绑定。然而flutter写桌面程序目前还很不成熟，官网写的几个例子跑出来的程序完全像在在电脑上装了个安卓模拟器里的界面，完全是照搬手机的界面，让人看到完全没有使用的欲望。


目前看来go语言gui库都还非常不成熟，期待有一天go语言能有一个好用的图形库！