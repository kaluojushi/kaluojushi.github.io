---
title: 解决 hexo-renderer-kramed 渲染冲突的部分问题
date: 2021-08-14 20:16:47
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

因 [hexo-renderer-marked](https://www.npmjs.com/package/hexo-renderer-marked) 不支持数学公式的渲染，其他渲染器又有一些问题，如 [hexo-renderer-pandoc](https://www.npmjs.com/package/hexo-renderer-pandoc) 过于沉重，[hexo-renderer-markdown-it](https://www.npmjs.com/package/hexo-renderer-markdown-it) 对NexT主题支持不佳，因此选用 [hexo-renderer-kramed](https://www.npmjs.com/package/hexo-renderer-kramed) 渲染器。本文解决了该渲染器在渲染 Markdown 及数学公式时遇到的部分问题。

<!--more-->

## 1 hexo-renderer-kramed 不能渲染 Todo List

原来的渲染器 hexo-renderer-marked 是支持的，那就翻一下 [hexo-renderer-marked的GitHub仓库](https://github.com/hexojs/hexo-renderer-marked) 的 Pull Request。

发现在这个 [PR](https://github.com/hexojs/hexo-renderer-marked/pull/32) 里，hexo-renderer-marked 加入了对 Todo List 的支持，那就拷贝这个PR中 `lib/renderer.js` 里新增的代码：

```javascript
// Support To-Do List
Renderer.prototype.listitem = function(text) {
  if (/^\s*\[[x ]\]\s*/.test(text)) {
    text = text.replace(/^\s*\[ \]\s*/, '<input type="checkbox"></input> ').replace(/^\s*\[x\]\s*/, '<input type="checkbox" checked></input> ');
    return '<li style="list-style: none">' + text + '</li>\n';
  } else {
    return '<li>' + text + '</li>\n';
  }
};
```

加入到本地的 Hexo 根文件夹的 `/node_modules/hexo-renderer-kramed/lib/renderer.js` 的第 19 行中。

保存后重新 `hexo clean`、`hexo g`，就渲染成功了。

其实这个解决方案不是我翻 PR 找到的，而是我在 GitHub 乱搜索时 [在这里](https://github.com/wafer-li/wafer-li.github.io/blob/source/src/blog-corners/tech/tinkering/hexo/Hexo%20Experience.md) 找到的。本来并不指望解决这个问题的，又学到一招。

## 2 hexo-renderer-kramed 渲染 MathJax 时与 Markdown 语法冲突

关于如何修改语义冲突，网上的教程讲得很详细了，比如：

- [hexo下LaTeX无法显示的解决方案 - zealscott - 简书](https://www.jianshu.com/p/d95a4795f3a8)
- [解决hexo-next主题和mathjax下划线冲突问题 | 拾荒志](https://murphypei.github.io/blog/2019/03/hexo-render-mathjax.html)

但网上找到的教程通常只讲述了如何修改 `_` 和 `\\` 的冲突。我将我遇到的所有问题一一整理如下。

下文的问题通常是行内公式的问题，目前看来公式块受到的影响比较小。

### 2.1 行内公式与行内代码冲突

**问题描述**：把行内公式作为行内代码输入时（如下），会显示异常，即 <code>`$</code> 被转义成 <code>$</code> 了。

```markdown
`$ a+b $`
```

翻一下 [hexo-renderer-kramed 的文档](https://github.com/sun11/hexo-renderer-kramed)，发现作者写在这里了：

![](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/20210814-06.png)

所以要想输入行内代码中的公式，在 `$` 前后加上空格就行了：

```markdown
` $ a+b $ `
```

### 2.2 下划线 `_` 被转义为斜体而非 LaTeX 下标

**问题描述**：当公式中出现多个下划线时，会被 kramed 渲染为 Markdown 斜体，导致公式显示异常。

Markdown 本身的语法是支持 `*` 和 `_` 都被转义为 *斜体* 的，所以我们可以取消掉 kramed 对 `_` 的转义。

打开本地 Hexo 根文件夹下的 `/node_modules/kramed/lib/rules/inline.js`，找到第 20 行如下代码：

```javascript
em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

修改为：

```javascript
em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
```

就取消了对下划线 `_` 的转义。

以后使用斜体的话只用 `*` 符号就够了。如果 LaTeX 要使用大量 `*` 符号，可用 `\ast` 代替。

### 2.3 反斜杠 `\\` 被转义为 `\` 而非 LaTeX 换行

**问题描述**：当公式中出现 `\\` 表示换行时，会被 kramed 渲染为 `\`，导致公式显示异常。

取消掉对 `\\` 的转义就行了。

同上，找到 `inline.js` 中第 11 行如下代码：

```javascript
escape: /^\\([\\`*\[\]()#$+\-.!_>])/,
```

修改为：

```javascript
escape: /^\\([`*\[\]()#$+\-.!_>])/,
```

就取消了对反斜杠 `\\` 的转义。

### 2.4 LaTeX 紧贴符 `\!` 不被转义

**问题描述**：当公式中出现 `\!` 表示紧贴符号时，会被 kramed 渲染为 `!`，导致公式显示异常。

同上，把 `escape:` 后的正则表达式中的 `!` 去掉即可，取消掉对 `\!` 的转义。

### 2.5 反斜杠加竖线 `\|` 被转义为 `|` 而非 LaTeX 双竖线

**问题描述**：当公式中出现 `\|` 表示紧贴符号时，会被 kramed 渲染为 `|`，导致公式显示异常。

真是个困扰了半天的 bug，上面的 `escape:` 后面也没有 `|` 啊。

找了我 n 个小时，原来还是 `inline.js` 代码的问题。

找到第 64 行如下代码：

```javascript
escape: replace(inline.escape)('])', '~|])')(),
```

修改为：

```javascript
escape: replace(inline.escape)('])', '~])')(),
```

就取消了对 `\|` 的转义。

执行完以上步骤后，记得 `hexo clean`、`hexo g` 走一波。

**总结：哪里被 kramed 转义就检查 `escape` 对应的部分。**

## 3 总结

- 多上 GitHub 看看原仓库的文档、issues、PR，会有收获的，实在不行直接搜，比百度好使。
- 熟悉流程，**Hexo 的原理是根据渲染器，把 Markdown 语法转为 HTML 语法。** 所以一些显示 bug 不一定是主题的问题，而很有可能是渲染器的问题。所以看看渲染器的源码总是有收获的。
