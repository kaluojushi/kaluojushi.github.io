---
title: 如何在一周内建造核心舱
date: 2021-08-03 03:15:51
categories: Myself
tags:
  - 博客
  - Hexo
  - 核心舱
  - 2021
comments: true
---

最近这段时间，我萌发了搭建一个技术博客的想法。仅仅用了一周左右的时间，我就搭建好了，并且把它取名为「卡洛的核心舱」。

<!--more-->

## 1 开始

在本学期，我开始逐渐投入到科研项目中。写代码时自然会遇到不少问题，绝大多数都是通过百度搜到他人的 CSDN 文章和个人博客文章解决了。少数问题我也是通过不断地尝试，自己排除了 bug。在这期间，部分十分宝贵的值得留存的解决方案我没有记录下来，十分可惜。

在项目暂告一段落后，我有了相对充足的一段时间，考虑到之前的自媒体平台不适合撰写技术文章，于是在研一的最后一个星期，我开始尝试搭建起技术博客。

目前主流的博客搭建方法有两种。一是 [WordPress](https://cn.wordpress.org/) + 云服务器的模式，这种方法我尝试过，但是随着我的腾讯云云服务器学生优惠机会所剩无几，而我也可能无法承担起如此高昂的租赁费用；此外它是基于 CentOS 和 PHP 开发，对我来说不大友好，有一定的学习成本。

二是利用 GitHub 的 [GitHub Pages](https://pages.github.com/) 功能，加上开源的博客框架，做出静态页面即可。我选择尝试这种方法。

在此之前，我参考了很多人的博客，包括本科同学 [哈哈哥的博客](https://leiwei.xyz/) 就是基于 GitHub Pages 搭建。

## 2 GitHub Pages 和 git

7 月 26 日，杭州的台风天还没结束，我冒着风雨到达办公室，正式开始搭建之旅。

我最开始试验的是 **GitHub Pages**。之前本科同学得意哥就用这个功能写了课设代码的前端页面。GitHub Pages 就是 GitHub 为用户提供的一个把代码转化为静态页面的一个工具，因此拿它写博客也不在话下。

这里值得一提的是，得益于我半年来做科研项目的过程，我对 **「用git管理代码」** 的方式已经非常熟悉，众多的 `git` 指令背了又背，`pull`、`push`、`origin` 之类的单词已经深入我心。它作为代码管理和版本控制的工具，虽然对于初学者可能劝退，但我却省去了学习的过程，根本不需要翻过第一座大山。

其实我到办公室最开始想尝试的是 Gitee Pages（科研项目用的是 [Gitee](https://gitee.com/)，国内的基于 `git` 管理代码的网站），但当我建好仓库，传好 HTML 时，却发现：

![绿色网络环境改造？？？](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210803-01.png)

于是我转向 GitHub Pages，重复过程搭建仓库后（<https://github.com/kaluojushi/gyhnb>），得到了我的 GitHub Pages 页面：

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210803-02.png)

链接是 <https://kaluojushi.github.io/gyhnb>。

这个 HTML 虽然简单，除了上面的 ~~nb~~ 文字外，下面甚至是 Lorem ipsum（乱数假文），但这是我 4 个月前，刚开始学习 HTML 时写出的第一个页面。

## 3 域名

我之前就注册过域名，所以也非常容易地打破了这个我小时候认为的技术壁垒。

为了注册一个合适的域名，我想要先起好博客的名字。很快啊，我想到一个非常极客的名字—— **「核心舱」**，并且在 7 月 28 日注册了 [corecabin.cn](https://corecabin.cn/)。

> 对于空间站而言，[核心舱](https://baike.baidu.com/item/天和核心舱)（Core Cabin）是 **整个空间站的中枢和管理控制中心**。与此类似，我的核心舱主要存储的是与「程序」「技术」「研究」或是「音乐」等一些很酷的东西。

绑定域名非常简单，也不需要域名备案，网上有现成的教程。于是在完成 `CNAME` 的上传后，过了 10 分钟，我就可以通过 [corecabin.cn](https://corecabin.cn/) 而不是 [kaluojushi.github.io](https://kaluojushi.github.io/) 去访问这个仓库的 pages 了。

## 4 Hexo

7 月 29 日这天有 6 场乒乓球赛。这一天，我不仅看完了每一场球赛，还基本完成了「核心舱」的搭建。

以 GitHub Pages 为基础的博客框架有三种，分别是 Jekyll、Hexo 和 Hugo，其中前两种用得比较多。当我看到 Jekyll 的全英文文档以及有点复杂的环境配置，我人晕了，于是劝退开始尝试 Hexo。

**Hexo** 是台湾人写的，因此有比较详细的中文文档，而且环境配置比较简单，只要 **Node.js 和 git** 即可。

不得不说，再次得益于做科研，这个环境我的电脑上已经有了。

![我的科研项目开发工具文档，真是好用啊](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210803-03.png)

虽然中间出现了 `'hexo'不是内部或外部命令，也不是可运行的程序或批处理文件。` 这样愚蠢的错误，但我很快知道这是环境变量的 `PATH` 的问题，很快设置好了。hexo 的命令也很好记，难度不大，在跟着教程配置一遍后，基本搭建就完成了。

搭建过程中，我觉得 **Hexo还是很香的**，不仅支持本地运行环境，还支持一键部署，并且用到的知识或工具，比如 `git`、`npm`、Markdown 等全是打过很多次交道的很熟悉的东西了，所以我很顺利，没有遇到什么 bug。

基础工作既然已经完成，后面的几天里，主要就是做一些完善工作了。

- 选了个主题 [maupassant](https://github.com/tufu9441/maupassant-hexo)，完成了下相关的配置；
- ~~部署了 [hexo-generator-search](https://github.com/PaicHyperionDev/hexo-generator-search) 搜索插件，可以实现文内搜索；~~
- ~~部署了 [hexo-wordcount](https://github.com/willin/hexo-wordcount) 插件，可以显示文章字数与阅读时间，如本文开头所示；~~
- 部署了 [Gitalk](https://github.com/gitalk/gitalk) 评论插件，可以用 GitHub 账号登录留言，如本文文末所示；
- 基于 GitHub Repository、[PicGo](https://github.com/Molunerfinn/PicGo/releases) 和 [jsDelivr](https://www.jsdelivr.com/) 搭建了个稳定的图床，本文所有图片均来自图床；
- 用 Markdown 写了一部分文章，以及一个 Markdown 文档。之前我就用 Markdown 写文档了，所以也不需要一个学习 Markdown 的过程；
- 调整了页面很多的细节，补充了一些页面。

## 5 总结

核心舱搭建成功了，还有些没整完的小细节可以慢慢弄。虽然这个过程中用到的都是我学过的知识，很少碰壁，但是我还是从中收获了很多：

- 熟悉了 ~~全球最大同性交友网站~~ GitHub，以后会常用；
- 进一步熟悉了 `git`；
- 进一步熟悉了 `npm`；
- 进一步熟悉了环境变量 `PATH`；
- 进一步熟悉了 Markdown；
- 进一步熟悉了前端知识。

一周时间过去，这一周我除了看看奥运比赛，从学校结束科研回家，也就在全身心投入这个事情了。但我丝毫没觉得时间被浪费，因为把时间投入自己喜欢做的事情，花再多精力也值得，尤其是我从中学习的知识、得到的收获、学会的 **开源精神** 和进一步的 **对代码的热爱**，这些不通过自己鼓捣自然是学不到的。

## 2021.8.13 更新

回家后的这段时间里，我也没闲着，继续鼓捣自己的核心舱，这个过程中也发现了众多的bug，并想办法排除了它们。

- 从主题 [maupassant](https://github.com/tufu9441/maupassant-hexo) 换成了主题 [NexT](https://github.com/theme-next/hexo-theme-next)，作为一个发展时间很长、被全网使用最多的、最为成熟的主题，它集成了很多插件，使得我不需要修改源代码；
- 修改了一下 favicon 及其路径，新增了 custom-logo，显示在电脑端右上角；
- 针对 NexT 主题侧边栏不显示分类与标签，新增了 [分类](\categories) 与 [标签](/tags) 页面，便于检索文章；
- 部署了 [hexo-symbols-count-time](https://github.com/theme-next/hexo-symbols-count-time) 插件显示文章字数与阅读时间，如本文开头所示；
- 修改了 Hexo 渲染 Markdown 的引擎，由 [hexo-renderer-marked](https://www.npmjs.com/package/hexo-renderer-marked) 改为 [hexo-renderer-kramed](https://www.npmjs.com/package/hexo-renderer-kramed)，以及修改了部分代码，以便对数学公式的渲染达到更好的支持，可以参考这篇文章：《{% post_link solve-some-problems-of-hexo-renderer-kramed-rendering-conflicts %}》；
- 部署了 [busuanzi](http://ibruce.info/2015/04/04/busuanzi) 插件，用来显示文章阅读次数与网站浏览次数，如本文开头、本站底部所示；
- 部署了 [hexo-generator-searchdb](https://github.com/theme-next/hexo-generator-searchdb) 插件，可以实现文内搜索，如侧边栏所示；
- 解决了中文目录不能跳转的 bug；
- 解决了 hexo-renderer-kramed 不能渲染 Todo List 的 bug，同样可以参考这篇文章：《{% post_link solve-some-problems-of-hexo-renderer-kramed-rendering-conflicts %}》；
- 解决了手机端总是不能访问核心舱的 bug，可以参考这篇文章：《{% post_link solve-the-problem-that-mobile-terminal-of-hexo-blog-cannot-be-accessed %}》；
- 通过配置 swig 文件的方式更新了友情链接的展现形式；
- 修正了因 Github RSA SSH 策略调整导致的无法部署的问题；
- 将开发环境由 Windows 切换到 Mac；
- 修正了所有正文的中英混排问题；
- ……

以后也可以把更新 bug 的过程写在这篇文章里。

## 参考 ~~文献~~ 教程

1. [Hexo官方文档](https://hexo.io/zh-cn/docs/)
2. [使用hexo+github搭建免费个人博客详细教程-好记的博客](http://blog.haoji.me/build-blog-website-by-hexo-github.html?from=xa)
3. [GitHub+Hexo 搭建个人网站详细教程 - 知乎](https://zhuanlan.zhihu.com/p/26625249)
4. [Github个人博客：绑定域名_heimu24的博客-CSDN博客](https://blog.csdn.net/heimu24/article/details/81159099)
5. [个人博客搭建笔记----hexo根目录下的\_config.yml配置解释_zem-CSDN博客](https://blog.csdn.net/zemprogram/article/details/104288872)
6. [大道至简——Hexo简洁主题推荐 | 屠城](https://www.haomwei.com/technology/maupassant-hexo.html)
7. [Hexo搭建博客以及多端同步更新_Kevin的专栏-CSDN博客](https://blog.csdn.net/a565102223/article/details/61926629)
8. [Github+jsDelivr+PicGo 打造稳定快速、高效免费图床\_TRHX'S BLOG-CSDN博客](https://blog.csdn.net/qq_36759224/article/details/98058240)
9. [NexT主题使用文档](http://theme-next.iissnan.com/)
10. [解决hexo-next主题和mathjax下划线冲突问题 | 拾荒志](https://murphypei.github.io/blog/2019/03/hexo-render-mathjax.html)
11. [hexo-theme-next的PR：fix: Chinese TOC cannot jump](https://github.com/theme-next/hexo-theme-next/pull/1540/commits/ec521c927dc10255977324284c1c667f2e220da7)
12. [hexo-renderer-marked的PR：Add Markdown List Support](https://github.com/hexojs/hexo-renderer-marked/pull/32)
13. [Hexo NexT主题自定义友链页面 | Leaface](https://www.liaofuzhan.com/posts/1123041323.html)
14. [HEXO部署博客内容到github报错_部署hexo时someone could be eavesdropping on you right-CSDN博客](https://blog.csdn.net/qq_42692386/article/details/129973669)
15. [git@github.com:Permission denied(publickey).fatal: Could not read form remote repository错误_git@github.com: permission denied (publickey). fat-CSDN博客](https://blog.csdn.net/zhan13253807836/article/details/122896326)

