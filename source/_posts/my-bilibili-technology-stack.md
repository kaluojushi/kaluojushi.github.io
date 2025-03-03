---
title: 作为 B 站 up 主，我使用的技术栈
date: 2025-03-03 01:06:59
categories: Bilibili
tags:
  - 个人项目
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

之前的文章提到过，我最开始在 B 站上传视频，就是发布苏打绿歌曲的伴奏。UVR5 就是我用来提取伴奏的工具。这个工具对音乐的处理极好，比网上或者 KTV 那种消音伴奏、单声道伴奏效果好多了。

我目前使用的模型主要是两个。需要带和声的伴奏，我使用的模型是 **VR Architecture** 的 **6_HP-Karaoke-UVR** 模型。需要去除所有人声只留乐器的伴奏，我使用的模型是 **MDX-Net** 的 **UVR-MDX-NET Inst Main** 模型。

目前在我使用的过程中，UVR5 也存在一些缺点，如下：

- 毕竟是从混音音轨提取，所以可能会有一些瑕疵，比如会带上喘息、气口，或是乐器不够突出等
- 对二胡等中国传统乐器的支持不太好，模型会认为是人声
- 对男女对唱或多人合唱的处理效果不太好，这种时候我会建议观众使用器乐伴奏

### 2.2 [Adobe Audition](https://www.adobe.com/products/audition.html)

## 3 图片获取

### 3.1 [Facebook](https://www.facebook.com/)

### 3.2 [Instagram](https://www.instagram.com/)

### 3.3 [小红书](https://www.xiaohongshu.com/)

### 3.4 [iSearch 4.5](https://i.oppsu.cn/)

## 4 图片处理

### 4.1 [美图秀秀](https://www.meitu.com/)

## 5 视频获取

### 5.1 [Youtube](https://www.youtube.com/)

### 5.2 [Bilibili](https://www.bilibili.com/)

## 6 视频处理

### 6.1 [剪映专业版](https://www.capcut.cn/)