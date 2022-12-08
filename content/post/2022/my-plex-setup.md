---
title: "利用阿里云盘和plex，打造属于自己的在线流媒体"
date: 2022-12-07T23:55:45Z
lastmod: 2022-12-07T23:55:45Z
draft: false
keywords: [“plex”, "aliyun", "streaming media"]
description: ""
tags: 
#  - 编程
#  - 摄影
  - 生活
  - 折腾
categories: 
#  - 编程
#  - 摄影
  - 生活
author: "simon"
---

出差这段时间，自己的时间多了一些。最近利用这段时间，打造了一个自己的在线流媒体服务。使用效果还挺好，分享一下我的搭建方法

这件事情的起因是因为了解到阿里云盘可以挂载成webdav形式，例如通过这个项目 [aliyundrive-webdav](https://github.com/messense/aliyundrive-webdav) 可以让阿里云盘轻松支持webdav，我们都知道webdav是一个开放的协议，有了webdav支持，就可以做很多其他事情。

后来有了解到了一个更强大的项目 [alist](https://github.com/alist-org/alist)，不紧支持阿里云盘webdav，还支持其他乱七八糟一系列网盘。下面是官网列出的一直支持的网盘

![alist](https://img.simonding.com/2022/alist-backend.png)

alist不仅支持的网盘多，而且有web界面，还支持aria2离线下载。可以说功能非常全面了。

有个这个能使阿里云盘支持webdav的东西，我们就可以利用它做很多事了。alist部署在VPS上，然后再用rclone挂载成本地磁盘，然后plex直接指定rclone挂载的路径就行了，我们就拥有了一个在线流媒体服务，可以播放阿里云里的所有资源。

如果想加速世界各地的访问，还可以给plex套上cloudflare，所有流量都走cdn，而且还是免费的，爽歪歪！

![plex](https://img.simonding.com/2022/plex-my.png)

这套搞成功之后还是挺舒服的，配合plex pass可以在各种客户端观看阿里云盘里的资源，还可以分享给女朋友利用plex的共同观看功能一直看，异地恋神器了有没有！

**中间遇到的一些坑**

1. alist离线下载功能会丢失目录

    这个不算什么大的bug，但是挺恼人的，导致我不太愿意用alist的离线下载功能。在看了看alist代码之后，发现这个问题也挺容易解决的，于是给项目提了个PR [fix(aria2) #1856, solve directory issue](https://github.com/alist-org/alist/pull/2504)，把这个问题解决了。最新版应该都不存在这个问题了。

2. plex无法从视频中间开始继续播放，每次都要重头开始，虽然plex能记住进度，但是每次都无法成功恢复进度，而且视频播放的时候进度条无法正常拖动

    这个问题挺讨厌的，每次看到一半，下次还要从头开始看，而且从头开始看进度条还不能拖动，属实让人头大了。这个问题搞了好长时间都不知道为啥，知道有一天在外网搜到了一个类似的问题，原来是cloudflare代理的问题导致的。*Caching* -> *Configuration*里面的*Caching Level*设置，不能用*Standard*，我试了下用我用*No query string*一切正常，用*Ignore query string*会导致移动端播放不了


解决了这两个问题之后，这个服务才算真正好用。这样一般看电影分为两种方式：

 * 直接找阿里云盘分享，保存到阿里云盘里，plex刷新一下就能看到电影了。
 * 自己找种子，利用alist的离线下载功能，由于服务器在国外，离线下载功能非常快，几乎全程10M/s+，一部电影几分钟就能下完，下完plex刷新下就能看到资源了，可以说是非常爽了

 这套软件大大简化了之前看电影了流程，基本上属于想看什么就立马能看到了，除了还有一个找资源的过程，不过大部分热门资源也很好找。

 这个方案基本上可以做到1080p不卡，不用忍受国内视频网站阉割过后的电影，而且清晰度、流畅度也非常可以，目前可以说非常满意了。