---
title: 解决Hexo博客手机端无法访问的问题
date: 2021-08-14 14:32:11
categories: Hexo
tags:
  - 博客
  - Hexo
  - 技术
  - 技巧
  - bug
  - GitHub
comments: true
---

**问题描述**：已经部署到GitHub Pages的Hexo博客，电脑端可以根据域名访问，手机端无法访问。

<!--more-->

### 1 原因

通常是GitHub Pages的ip地址解析出现的bug。电脑端可以正常访问大概率是因为科学上网的缘故，手机端无法访问是因为拒绝了域名解析所用的ip。

### 2 解决方法

#### 2.1 重新ping一下github.io的ip

打开cmd，输入以下指令：

```shell
ping kaluojushi.github.io
```

`ping`后面跟GitHub Pages的地址，如`username.github.io`。

可以看到当前的ip是185.199.109.153。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-01.png)

#### 2.2 修改域名解析

打开域名DNS解析商（我的是腾讯云），修改A记录的记录值为上面ping到的ip。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-02.png)

#### 2.3 验证

打开[站长工具DNS查询](http://tool.chinaz.com/dns/)，选择A类型，输入域名，查看解析结果，如果为刚才修改的结果就表示成功（如果未成功需要等待一段时间）。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-03.png)

至此电脑端和手机端应该都可以访问了。

![电脑端](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-04.png)

![手机端](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-05.png)

### 3 总结

此问题其实与Hexo无关，还是国内访问GitHub及其所提供的服务不稳定的bug。再有问题就再换A记录！

不知道一条域名是否可以添加多条A记录，没有深入探究，在此暂时不表。

用[Gitee Pages](https://gitee.com/help/articles/4136)代替GitHub Pages其实也是备选项之一，对国内服务的支持更好。
