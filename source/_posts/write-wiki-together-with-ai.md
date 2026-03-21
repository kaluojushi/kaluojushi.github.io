---
title: 和 AI 一起写知识库
date: 2026-03-16 00:43:47
categories: Project
tags:
  - AI
  - 个人项目
  - 编程
  - 知识库
  - 技术
  - 核心舱
---

之前说今年要多做个人项目，今年的第一个个人项目还是偏向于给我自己用的项目，就是做了个个人知识库。神奇的是，知识库内容的书写是我和 AI 一起完成的。

<!-- more -->

## 1 Corecabin Wiki

{% post_link my-2025-annual-summary '2025 的年度总结'%} 里说，今年要做更多个人项目，也要掌握一些新的技术，于是我迫切地需要做一个个人知识库，来记录我的一些技术积累和学习笔记，刚好这就是我今年的第一个个人项目了。

知识库叫 [**Corecabin Wiki**](https://wiki.corecabin.cn/)，网址是 [wiki.corecabin.cn](https://wiki.corecabin.cn)。

首先在技术选型上，我首先是参考了本科同学 [哈哈哥的个人知识库](https://leiwei.space/wiki/)，他使用的是框架是 [TiddlyWiki](https://tidgi.fun/)。不过说实话这种页面模式我不太喜欢，这种标签式的卡片一个个蹦出来，一下子就迷失方向了（~~现在我也没看懂哈哈哥的 wiki 是怎么写的~~）。后来还是使用了之前做 <a href="https://corecabin.cn/carlo-tools/" target="_blank">Carlo Tools</a> 时用过的 [**docsify**](https://docsify.js.org/#/zh-cn/)。这种目录树的形式也比较直观，好组织内容。

## 2 静态页面部署

由于 docsify 是个轻量级文档生成器，所写的 Markdown 文件不需要构建静态 HTML 文件，而是在运行时自动转换，所以可以直接扔在 GitHub Pages 上，也不需要新建分支，Carlo Tools 就是这么干的。而且在不配置自定义域名的情况下，Carlo Tools 的访问地址就是 `kaluojushi.github.io/carlo-tools/`（因为 `kaluojushi.github.io` 被我映射成了核心舱地址 `corecabin.cn`，所以 Carlo Tools 实际上的访问地址是 `corecabin.cn/carlo-tools/`）。

但上次说到我想试试 [**Vercel**](https://vercel.com/) 的静态页面部署，于是打算把 Corecabin Wiki 部署在 Vercel 上。Vercel 的部署方式和 GitHub 一样简单，选择仓库、设置构建命令（docsify 不需要构建，所以直接留空）和输出目录（docsify 的输出目录是 `./docs`），就可以完成部署，然后就可以用 [corecabin-wiki.vercel.app](https://corecabin-wiki.vercel.app) 去访问了。

考虑到 Vercel 的自定义域名配置超方便，我就给 Corecabin Wiki 的配置了核心舱的二级域名 `wiki.corecabin.cn`。Vercel 配置的自定义域名在国内访问似乎比 Github Pages 更快一些，不知道是什么原因，可能是 GitHub 太容易被墙了？以后如果做那种面向大众的项目，可能会更倾向于用 Vercel 来部署了。

> 比如我很快就把我做的 [苏打绿 2024 二十年一刻观演报告](https://github.com/kaluojushi/20yike-2024-annual-report) 重新部署在 Vercel 上了，并配置了新域名 [2024.sodagreen.me](https://2024.sodagreen.me)。效果好得不得了。

docsify 其实还支持使用 [Vercel CLI](https://vercel.com/docs/cli) 来部署，直接在本地运行 `vercel` 命令就行，这个还没试过，以后可以尝试一下。

## 3 AI 辅助完成 wiki

上次说过今年的个人项目可能会更多借助 AI 来辅助完成，没想到先在个人知识库用上了。

一开始我就想将 Vercel 项目配置自定义域名的这个步骤写到 wiki 里，但我发现我写了个简单的步骤后，就不知道怎么扩充了。于是我让 [OpenCode](https://opencode.ai/) 帮我扩展文档，结果它直接写成了一篇超详细的文档（~~简直是个互联网裁缝~~），远远超出我的预期，甚至把常见问题补充得明明白白。于是，以同样的步骤，我写大纲，它来扩充，就完成了好几篇 wiki，效率极其之高。

后来我发现，虽然 OpenCode 写的文档确实很详细，但它并不符合一个个人知识库风格，有这些问题：

- 它把文档理解成了指南文档而不是笔记，例如「本指南」「本教程」的描述，这不是给外人看的，个人知识库更应该是告诉我自己应该怎么做
- 文档中有很多第二人称表述，如「你的 GitHub 用户名」，但我只需要知道这里使用用户名就可以
- 文档中有很多细节描述，这对我来说是十分冗余的，一些过于简单的前置条件和复杂的操作步骤我都是不需要知道的

当我把这些信息喂给 OpenCode 之后，它马上开始大刀阔斧的删减，最终的结果就完全符合我的预期了。为了让它保持这个风格，我又让它把这个风格写到 [AGENT.md](https://github.com/kaluojushi/corecabin-wiki/blob/main/AGENT.md) 和 [AGENT_ZH.md](https://github.com/kaluojushi/corecabin-wiki/blob/main/AGENT_ZH.md) 里。记住这个最核心的原则后，它不仅可以修改自己写的文档，应该以后也可以把我自己写的文档修改成这个风格。

## 4 做 favicon

以前的 favicon 都是我自己设计的，要么通过找图片来抠图（比如核心舱），要么通过 icon 素材网找现成素材（比如 Carlo Tools），然后通过工具修改尺寸和格式，于是开始一直没有做 Corecabin Wiki 的 favicon。

今晚突然想，OpenCode 既然能写代码，那生成一个 SVG 应该没问题吧？于是我又让它阅读了我的整个项目，然后让它根据项目设计一个 favicon，结果它在询问了我想要的风格后，直接就生成了一个 SVG 文件。虽然在工作时我就见识过 AI 帮我写代码的强大能力，没想到它在几乎「全盲」的情况下，能够直接「看见」图片的样子，这让我感到太神奇了。

> 「全盲」是因为大模型毕竟是 LLM（大语言模型），并非多模态模型，所以它并不能真正「看见」图片，只是「想象」出图片的样子。我觉得我如果让它修改 SVG 的一部分样子，它应该也能做到。
> 
> 之前工作的时候还有一个能力让我感到很神奇，就是它能辅助写 ArkTS 代码，在没有编译的情况下，它也能「想象」出页面的渲染布局效果，能根据我的描述修改细节，太 nb 了。

以下就是 OpenCode 生成的 SVG 文件，我一点都没有参与设计，大家看看效果如何，反正我觉得对一个 favicon 来说已经足够了：

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" width="512" height="512"><defs><linearGradient id="bgGradient" x1="0%" y1="0%" x2="100%" y2="100%"><stop offset="0%" style="stop-color:#667eea"/><stop offset="100%" style="stop-color:#764ba2"/></linearGradient><linearGradient id="bookGradient" x1="0%" y1="0%" x2="100%" y2="100%"><stop offset="0%" style="stop-color:#ffffff"/><stop offset="100%" style="stop-color:#e8e8ff"/></linearGradient></defs><circle cx="256" cy="256" r="240" fill="url(#bgGradient)"/><circle cx="256" cy="256" r="208" fill="none" stroke="#ffffff" stroke-width="4" opacity="0.3"/><ellipse cx="256" cy="256" rx="224" ry="80" fill="none" stroke="#ffffff" stroke-width="3" opacity="0.25" transform="rotate(-30 256 256)"/><ellipse cx="256" cy="256" rx="224" ry="80" fill="none" stroke="#ffffff" stroke-width="3" opacity="0.25" transform="rotate(30 256 256)"/><g transform="translate(128, 160)"><path d="M128 48 L128 192 Q64 176 0 192 L0 48 Q64 32 128 48 Z" fill="url(#bookGradient)" opacity="0.95"/><path d="M128 48 L128 192 Q192 176 256 192 L256 48 Q192 32 128 48 Z" fill="url(#bookGradient)" opacity="0.95"/><line x1="128" y1="32" x2="128" y2="208" stroke="#667eea" stroke-width="4"/><line x1="24" y1="80" x2="104" y2="72" stroke="#764ba2" stroke-width="3" opacity="0.4"/><line x1="24" y1="112" x2="104" y2="104" stroke="#764ba2" stroke-width="3" opacity="0.4"/><line x1="24" y1="144" x2="104" y2="136" stroke="#764ba2" stroke-width="3" opacity="0.4"/><line x1="152" y1="72" x2="232" y2="80" stroke="#764ba2" stroke-width="3" opacity="0.4"/><line x1="152" y1="104" x2="232" y2="112" stroke="#764ba2" stroke-width="3" opacity="0.4"/><line x1="152" y1="136" x2="232" y2="144" stroke="#764ba2" stroke-width="3" opacity="0.4"/></g><circle cx="96" cy="112" r="8" fill="#ffffff" opacity="0.6"/><circle cx="416" cy="128" r="10" fill="#ffffff" opacity="0.5"/><circle cx="80" cy="384" r="6" fill="#ffffff" opacity="0.4"/><circle cx="432" cy="368" r="8" fill="#ffffff" opacity="0.5"/></svg>

此外，在我的要求下，它还帮我生成了高清 SVG 和使用于苹果设备的 apple-touch-icon。

在我让它帮我把图片转换为 PNG 格式时它就犯了难，似乎它并没有这个能力。但很快，它就开始尝试使用我电脑上的 npm，在本地装了一个临时工具 Sharp 库，然后快速写了个 js 脚本来操作。还别说，这比我自己去找工具转换方便多了。AI 的能力真是太强大了，感觉现在我也只触碰到了冰山一角，以后还可以继续探索一下。

> 关于「全盲」问题，我询问了大模型自己，它给我的回答是：
>
> 它的视觉能力是一种基于文本训练的伪视觉能力。对于示意图，它是基于它训练数据中的 ASCII 艺术；对于 SVG 代码，它通过学习的语法规则进行代码生成；对于前端或 ArkTS 渲染，它通过学习的代码和渲染结果的配对描述进行模式映射。它的能力本质上还是通过文本的统计学习，而不是视觉认知。

## 5 和大家汇报一下我的 AI 使用进展

从年初公司推行 AI 辅助编码以来，现在我已经形成了比较确定的工作模式，主力使用 OpenCode + 智谱提供的 GLM-5 模型，反复使用 Plan mode 和 Build mode 来完成工作任务，同时通过 GitHub Copilot 来补全代码，通过 DeepSeek 来整理知识。

今年公司要求我们用 AI 辅助编码要达到总编码的 50% 以上。在工作过程中，我还是探索出了一些神奇的用法：

- 写业务代码，包括需求代码和解问题代码，这个都没问题，但是要先做好 Plan 再写，不然会把代码改成一团糟
- 写单元测试代码，效率非常高，对我的函数逻辑理解非常好
- 写自测试 Demo，也就是 ArkTS 代码，比如 [这个仓库](https://gitcode.com/kaluojushi1/ImageContentChangeReport)，主要是根据我的需求规格分解测试点并完成 Demo 编写，效率很高
- 控制工程机，OpenCode 可以使用命令行工具，自然也可以通过 shell 去控制设备或是读取设备上的文件
- 根据文档安装环境，这个超厉害，给人看的环境配置文档，让 OpenCode 边读边做一下子就给我装好了，遇到问题还能自己 debug

其实还有很多其他用法，比如有同事用 AI 来改文档，有同事用 AI 来做架构优化，还有同事直接让它监督特定的编译任务，感觉其实还可以玩出花来。

个人项目上，我使用 AI 重构了 [苏打绿 2024 二十年一刻观演报告](https://github.com/kaluojushi/20yike-2024-annual-report)，主要是把之前 [laf](https://laf.run/) 上使用的云函数和云存储迁移到了本地（~~避免流量冲击让我老是给 laf 续费~~）。其他代码层面的就没什么了，主要还是在帮我写文档。

最后一个问题，目前这种工作情况下有什么坏处？其实是有的，最明显的是，遇到一个问题我可能很少会去思考了，反正 AI 会帮我思考；编码途中遇到了问题，我也不会去到处查看看为什么，反正 AI 会帮我解决掉，然后我就把这个过程忘掉。**Debug 其实是一个很重要的学习体验**，即使是前人踩过的坑我再踩一遍，也是能学到东西的。现在太依赖 AI 反而让我损失了一些独立思考能力。时刻记住，AI 是辅助工具，它能帮助提升效率，我的思考和学习才是我的能力。

那么就从核心舱的文章开始，自己的博客一定是自己写！
