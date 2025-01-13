---
title: 解决 Hexo 博客手机端无法访问的问题
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

**问题描述**：已经部署到 GitHub Pages 的 Hexo 博客，电脑端可以根据域名访问，手机端无法访问。

<!--more-->

### 1 原因

通常是 GitHub Pages 的 ip 地址解析出现的 bug。电脑端可以正常访问大概率是因为 ~~科学上网的缘故~~ 存在 DNS 缓存，手机端无法访问是因为拒绝了域名解析所用的 ip。

### 2 解决方法

#### 2.1 重新 ping 一下 github.io 的 ip

打开 cmd，输入以下指令：

```shell
ping kaluojushi.github.io
```

`ping` 后面跟 GitHub Pages 的地址，如 `username.github.io`。

可以看到当前的 ip 是 185.199.109.153。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-01.png)

#### 2.2 修改域名解析

打开域名 DNS 解析商（我的是腾讯云），修改 A 记录的记录值为上面 ping 到的 ip。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-02.png)

#### 2.3 验证

打开[站长工具 DNS 查询](http://tool.chinaz.com/dns/)，选择 A 类型，输入域名，查看解析结果，如果为刚才修改的结果就表示成功（如果未成功需要等待一段时间）。

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-03.png)

至此电脑端和手机端应该都可以访问了。

![电脑端](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-04.png)

![手机端](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-05.png)

### 3 总结

此问题其实与 Hexo 无关，还是国内访问 GitHub 及其所提供的服务不稳定的 bug。再有问题就再换 A 记录！

不知道一条域名是否可以添加多条 A 记录，没有深入探究，在此暂时不表。

用 [Gitee Pages](https://gitee.com/help/articles/4136) 代替 GitHub Pages 其实也是备选项之一，对国内服务的支持更好。
