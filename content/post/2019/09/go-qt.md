---
title: "Go 语言 QT 开发踩坑"
date: 2019-09-30T23:23:48+08:00
lastmod: 2019-09-30T23:23:48+08:00
draft: false
keywords: []
description: ""
tags: []
categories: ["编程"]
author: "simon"
---

最近为公司开发了一个工具来简化一些繁琐的操作。试用了很多个 GUI 库之后，最终转到了 qt 上。工具使用了 [therecipe/qt](https://github.com/therecipe/qt) 项目的qt绑定。

go语言有好几个qt绑定的库，但是大部分都已经不在维护了，之后这个 [therecipe/qt](https://github.com/therecipe/qt) 项目目前还在积极的维护。开发者也比较活跃。中间遇到了问题，在github提了个 issue 开发者也很快回复了。

但是这个库目前还处于积极开发的状态，文档奇缺。英文文档都很少，更别提中文文档了。在谷歌里搜索这个库，几乎找不到一个像样的demo的例子。

github主页上有详细的安装方法，但是由于中国的网络环境。安装过程并不是很顺畅。其他的基本上都要看作者给的代码示例了。

**我下面的坑都是在 Mac 环境下的，windows下可以做参考**

<!--more-->

## 坑一：安装

MacOS 上安装qt绑定，基本是按这个文档 [Installation on macOS](https://github.com/therecipe/qt/wiki/Installation-on-macOS)，不过中间有好多坑要注意。

### Xcode
需要安装Xcode，虽然官方给的教程好像用command line也可以，但是坑较多。需要改好多东西。我最后还是安装Xcode一次性解决。


### QT

安装qt按照 [Installation on macOS](https://github.com/therecipe/qt/wiki/Installation-on-macOS) 来就可以。注意的是homebrew安装需要设置环境变量 QT_HOMEBREW=true，如果你不需要iOS和Android的支持，安装homebrew版本即可。我个人目前使用的就是homebrew版本的qt。

截止目前，项目官方支持的版本是Qt 5.13.0，其他版本需要更改一些环境变量。我没有测试过。

### 安装 therecipe/qt 绑定

安装官方文档的安装方式，还算简单

```sh
go get -u -v -tags=no_env github.com/therecipe/qt/cmd/...
$(go env GOPATH)/bin/qtsetup
```

不过第一 go get 操作会下载一下 golang的包，自行做好命令行翻墙的准备。

到这一步，如果一切顺利的话，应该可以正常编译出几个测试程序。就可以开始写程序了。

## 打包和交叉编译

项目代码是没有任何代码使用方式的文档的，大部分时间要参照作者 examples目录下的示例代码。推荐使用go module来管理第三方依赖。

### 项目打包问题

写好代码之后，测试代码是否能正常运行，就要把代码打包成可执行性文件。具体打包操作是最坑的一点。

在我踩过无数坑之后，一个大概可以的打包流程如下：

* 下载依赖


```sh
go mod download 
```

*  由于中国特殊的网络环境，并不能按照官方的教程一步步来走。还好作者给出了一个解决办法 [more info here, such as how to work around the GFW](https://github.com/therecipe/qt/issues/755#issuecomment-455248057)。简单来说就是执行下面一条命令

```sh
go get -u -v github.com/therecipe/qt/cmd/qtsetup && go get -u -v github.com/therecipe/qt/cmd/...
```
* therecipe/qt 打包方式是使用vendor目录打包，而不是使用最新的 go mod。我们要把所有的依赖放到vendor目录下

```sh
go mod vendor
```

* 克隆环境依赖

```bash
cd ./vendor/github.com/therecipe && git clone https://github.com/therecipe/env_$(go env GOOS)_amd64_512.git && cd ../../..
```
**这一步挺重要的要不然会打包不成功**

* 打包

```sh

qtdeploy test desktop

#或者
qtdeploy build dektop
```

### 交叉编译问题

*---待填坑---*


