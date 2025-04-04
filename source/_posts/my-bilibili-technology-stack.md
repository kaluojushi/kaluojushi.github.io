---
title: 作为 B 站 up 主，我使用的技术栈
date: 2025-03-03 01:06:59
categories: Bilibili
tags:
  - 个人项目
  - 技术
  - 音乐
  - 苏打绿
  - Bilibili
comments: true
---

从 2022 年 11 月我上传第 1 个视频开始，我成为 B 站 up 主已经有两年多的时间了。期间也拥有了两千粉丝，并收获了二十多万的播放量。

本文整理了我作为 B 站 up 主所使用的一些「技术栈」，也就是各种软件、网站、工具等，供大家参考学习。

<!-- more -->

## 1 音频获取

### 1.1 [QQ 音乐](https://y.qq.com)、[网易云音乐](https://music.163.com/)

没想到吧，这俩是我用来下音乐资源最常用的软件，而不是其他音乐资源网。

因为我是绿钻+黑胶会员，所以我可以直接下载音乐文件。但是由于这两者下载的音乐文件是特定格式（QQ 音乐为 mgg 格式，网易云音乐为 ncm 格式），只支持会员期在本地播放，所以想要拿到能直接使用的音乐格式，还需要下一步的音乐解锁。

网易云音乐下载的音乐文件，会带上歌曲的专辑封面，这是比较好的一点。

### 1.2 [音乐解锁](https://demo.unlock-music.dev/)

网址为 <https://demo.unlock-music.dev/>。

该网站可用于解锁加密音乐文件，也就是 QQ 音乐、网易云音乐等软件下载下来的会员歌曲。

该网站的 [GitHub 仓库](https://github.com/unlock-music/unlock-music) 已经被 ban 了，现在他们使用 [自建 Git](https://git.unlock-music.dev/um) 来管理代码。

由于我现在使用 Macbook 作为日常生产力，所以我使用的是 [音乐解锁 React 版](https://um-react.netlify.app/) 进行音乐解锁。其中，QQ 音乐的版本应降级到 8.8.0，并在网站上传密钥，跟着网站提示操作就可以了。

## 2 音频处理

### 2.1 [UVR5](https://github.com/Anjok07/ultimatevocalremovergui)

仓库为 <https://github.com/Anjok07/ultimatevocalremovergui>。

UVR5 全称 Ultimate Vocal Remover，是一个用大模型提取伴奏的工具。

{% post_link my-2023-and-2024-annual-summary#3-2-苏打绿伴奏 '之前的文章'%} 提到过，我最开始在 B 站上传视频，就是发布苏打绿歌曲的伴奏。UVR5 就是我用来提取伴奏的工具。这个工具对音乐的处理极好，比网上或者 KTV 那种消音伴奏、单声道伴奏效果好多了。

UVR5 使用各种大模型来处理音乐，因此需要极大的空间（好像软件安装包就 1 个 G，更别说软件本体了）和超强的性能（支持 GPU 加速）。我当前使用的是 M4 Pro 芯片的 Macbook Pro，跑完一首歌大概只需要 10 秒钟，而我当时研究生期间，用办公室电脑的 CPU 跑时，一首歌需要 1-2 分钟，而且 CPU 占用率被拉到 100% 根本没法做别的事情。

<img src="https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20250303/20250303-UVR.png" alt="UVR5 页面" style="zoom:35%;" />

我目前使用的模型主要是两个。需要带和声的伴奏，我使用的模型是 **VR Architecture** 的 **6_HP-Karaoke-UVR** 模型。需要去除所有人声只留乐器的伴奏，我使用的模型是 **MDX-Net** 的 **UVR-MDX-NET Inst Main** 模型。

目前在我使用的过程中，UVR5 也存在一些缺点，如下：

- 毕竟是从混音音轨提取，所以可能会有一些瑕疵，比如会带上喘息、气口，或是乐器不够突出等
- 对二胡等中国传统乐器的支持不太好，模型会认为是人声
- 对男女对唱或多人合唱的处理效果不太好，这种时候我会建议观众使用器乐伴奏
- 大模型的下载需要科学上网

### 2.2 [Adobe Audition](https://www.adobe.com/products/audition.html)

Audition（简称 AU）是 Adobe 家专业的音频处理软件。我从高中就开始使用 AU 剪辑音频，虽然到现在开始，我还只掌握了点皮毛。

我使用 AU 主要的场景是用于录制「{% post_link my-2023-and-2024-annual-summary#3-3-苏打音趴 '苏打音趴'%}」节目，以及节目后的剪辑。主要涉及到我和另一位主持人的录音，需要相互剪辑为一个音轨，以及加上节目 BGM。其次，我有时候还会使用 AU 剪辑单个音乐文件。

我目前使用的 AU 也并不是破解版，而是浙大的正版。得益于我毕业前给我浙 VPN 充值数千块钱，我目前依然可以享受到在校生福利，足够我使用到 60 岁。

~~也许到我 60 那年发现自己充少了，因为还没退休~~

## 3 图片获取

### 3.1 [Facebook](https://www.facebook.com/)

Facebook 上主要用于获取苏打绿的一些动态图片。不从微博找主要是因为一是微博图片有水印，二是苏打绿微博开了半年可见，对于我这种需要经常考古的粉丝来说简直是灾难。

Facebook 的图片可以直接右键保存，没啥好说的。

### 3.2 [Instagram](https://www.instagram.com/)

Instagram 我会保存一些苏打绿团员的动态图片，但由于 Instagram 本身的限制，存取照片通常需要借助一些小程序。这一类小程序比较多，在此不做介绍。

### 3.3 [小红书](https://www.xiaohongshu.com/)

小红书和微博一样，是庞大的粉丝讨论社群，因此有很多粉丝发布的十分有价值的图片。但小红书直接保存的图片也带有水印。

在此介绍我所用的小程序，【小红图去水印小工具】，该小程序完全免费使用，除了使用一键保存功能时需要看广告。不过目前对评论区的图片，除了截图外，我目前还没有什么比较好的方法。

### 3.4 [iSearch 4.5](https://i.oppsu.cn/)

网址为 <https://i.oppsu.cn/>。

强烈推荐该网站！！！

这个网站其实只是个音乐搜索网站，使用的是 iTunes 的资源。但它最大的作用，**其实是可以保存专辑的高清封面**。只要是 Apple Music 上的专辑，都可以在该网站中找到对应的专辑封面，最高支持 10000×10000 的分辨率。（而 QQ 音乐中保存的专辑封面只有 300×300，低清得可怜）

![iSearch 4.5 页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20250303/20250303-iSearch.png)

## 4 图片素材

### 4.1 [Grabient](https://www.grabient.com/)

网址为 <https://www.grabient.com/>。

这是一个渐变色生成网站，可以生成各种渐变色，包括线性、径向、径向渐变等。

<span style="background:linear-gradient(160deg, #0093E9 0%, #80D0C7 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;filter:saturate(200%)">拿到渐变色后，就可以设置到各种图片或样式当中了（就像这段文字），也可以一键复制 CSS。</span>

### 4.2 [Coolors](https://coolors.co/)

网址为 <https://coolors.co/>。

Coolors 是一个在线配色工具，可以生成各种配色方案，当需要使用多种颜色时，使用这个网站就可以快速生成。

### 4.3 [Texturelabs](https://texturelabs.org/)

网址为 <https://texturelabs.org/>。

Texturelabs 是一个纹理类素材库，包含丰富的肌理种类，比如纸张、金属、墙面等。需要使用图片作为底纹时，就可以使用这个网站。

## 5 图片处理

### 5.1 [美图秀秀](https://www.meitu.com/)

美图秀秀的电脑版和手机版是我处理图片最常用的软件。我从小学就开始使用美图秀秀处理图片，相比 PhotoShop，我觉得它更简单易用。

此外，美图秀秀我也充值了会员，对于一些会员专属功能，还是有用的，而且电脑手机 VIP 互通，比较良心。而且在近年也更新了很多 AI 功能，十分好用，我最常使用的有 AI 消除、AI 抠图等功能。

### 5.2 视频转 GIF

有时候我需要将视频转为 GIF 图，这时候我会使用 iOS 自带的快捷指令 APP，使用内置的指令即可。不过我用的最多的还是【视频转 GIF】这个小程序，相比快捷指令，这个小程序可以实现调速、增加字幕等功能，同时也基本是免费的（要看一下广告）。

## 6 视频获取

### 6.1 [Youtube](https://www.youtube.com/)

Youtube 主要是用来下载苏打绿的 MV 视频。Youtube 下载视频的方式很多，Google 一搜一大把，这里有一大堆丰富的 [Youtube 视频下载网站](https://github.com/Alvin9999/new-pac/wiki/YouTube%E4%B8%8B%E8%BD%BD1080%E6%95%99%E7%A8%8B)，可以参考。

但是 Youtube 下载视频很难下到完整的 1080P 或 4k 分辨率的视频，目前还是没有什么更好的方法。

### 6.2 [Bilibili](https://www.bilibili.com/)

所以有时候我也会在 Bilibili 上找 MV 下载。B 站下视频方式也更多，我自己使用的是 [ACG 助手（原名哔哩哔哩助手）](https://acghelper.com/) 这个 Chrome 扩展。

## 7 视频处理

### 7.1 [剪映专业版](https://www.capcut.cn/)

对于剪视频来说，我使用剪映专业版。

最开始我使用的是 B 站官方出的 [必剪](https://bcut.bilibili.cn/)，虽然它与 B 站关联密切，但其实还是不如老牌视频剪辑软件——剪映好用。

剪映我也充值了会员（~~果然一直氪金一直爽~~），不过用到的会员功能并不多，基础的剪辑功能已经够用了。

![剪映页面](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20250303/20250303-capcut.png)
