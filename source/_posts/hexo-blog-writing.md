---
title: Hexo博客写作
date: 2021-08-03 04:04:20
categories: Hexo
tags:
  - 博客
  - Hexo
  - 技术
  - 技巧
comments: true
---

记录本博客的撰写过程，便于日后按图索骥。

<!--more-->

## 0 同步远程仓库（多端）

在当前电脑的`kaluojushi.github.io`文件夹（已`git init`和`remote`）下的`hexo`分支，打开`git bash`，依次执行：

```bash
$ git fetch --all
$ git push origin hexo
```

以同步远程仓库代码。

这个操作也可以通过IDE或[GitHub Desktop](https://desktop.github.com/)进行，而且更推荐！

## 1 清除缓存

根目录下执行：

```bash
$ hexo clean
```

清除`/public`、`/db.json`等缓存。可做可不做，但当页面改动较大时，或拉了代码时，建议做一下。

## 2 新建md文档

下述`hexo new`均可缩写为`hexo n`。

### 2.1 新建文章

根目录下执行：

```bash
$ hexo new [post] "postName"
```

根目录会在`/source/_posts`下新建文件`postName.md`，通常采用英文命名`postName`。

### 2.2 新建页面

根目录下执行：

```bash
$ hexo new page "pageName"
```

根目录会在`/source/pageName`下新建文件`index.md`。

若要生成二级页面，则根目录下执行：

```bash
$ hexo new page --path about/me "pageTitle" #--path可缩写为-p
```

根目录会在`/source/about`下新建文件`me.md`，并将`title`命名为`pageTitle`，**注意`title`必须指定**。

### 2.3 新建草稿

根目录下执行：

```bash
$ hexo new draft "draftName"
```

根目录会在`/source/_drafts`下新建文件`draftName.md`。

发表草稿时，用`publish`命令：

```bash
$ hexo publish [post/page] <fileName>
```

新建草稿一般不用，因为可以先写好Markdown然后再`new post`就好。

## 3 编辑Front-matter

Front-matter是文件最上方以`---`分隔的区域，一般来说完整应如下：

```markdown
---
title: postTitle #标题，默认为文件名，一般会自行更改为中文的
date: 2021/08/01 11:45:14 #建立日期
updated: #更新日期
categories: #分类（page不适用）
tags: #标签（page不适用）
type: #页面类型（仅page）
description: #摘要，详细见第4节
comments: true #评论，默认为true
layout: post #布局，默认为post
mathjax: false #LaTeX公式，默认为false
---
```

### 3.1 标题

hexo会把Markdown文档转化为html静态网页，因此要想实现文章标题与域名不同，可以通过以下方法

- 文件名为`英文标题.md`的形式。*根据谷歌文档，建议使用`-`分隔单词，而不是`_`或驼峰式。*
- Front-matter的`title`为实际的中文标题。

这样文章域名就为`英文.html`，而标题为中文。

### 3.2 日期

- `date`：建立日期，自动生成，默认为Markdown文档建立日期。
- `updated`：更新日期，不自动生成，但NexT主题会自动读取文档修改日期；可以自行添加或修改。

### 3.3 分类

单分类很简单，直接跟后面就行：

```markdown
categories: Diary
```

表示分类为Diary。

子分类用逗号表示，例如：

```markdown
categories: [Diary, Life]
```

表示分类为Diary的子分类Life。*别忘了方括号，否则会被理解为一个分类，名为“Diary, Life”。*

并列分类则使用列表形式，例如：

```markdown
categories:
	- [Diary]
	- [Life]
	- [Music, Piano]
```

表示分类分别为“Diary”、“Life”和“Music的子分类Piano”这三个分类。

~~好麻烦，所以应当提前有一个大体的分类思路。~~

### 3.4 标签

单标签也是直接输入就行。

多标签有以下两种表示方法：

```markdown
tags: [C, Java, python]
```

或

```markdown
tags:
	- C
	- Java
	- python
```

**注意：设置分类、标签列表**

打开根目录的`_config.yml`，对下面代码进行更改：

```yaml
# Category & Tag
default_category: uncategorized
category_map:
	编程: programming
	生活: life
	其他: other
tag_map:
```

则可以更改对应分类、标签的访问路径。

### 3.5 页面类型（NexT主题）

对于page，可以设置`type`为有关类型：

- `type: categories`：分类页面。
- `type: tags`：标签页面。
- `type: links`：友链页面，为自己添加，参考[此文](https://www.liaofuzhan.com/posts/1123041323.html)。

### 3.6 评论

将`comments`设置为`true`即可，默认即为`true`，所以可以不加这一条。如果要关闭评论，则设置`false`即可。

page的`comments`也是默认开启的，包括categories页面和tags页面！！！

```markdown
comments: true
```

### 3.7 `layout`

对于post，`layout`默认为`post`，因此一般不加。

对于page，建议是在new完后，在`index.md`的Front-matter加上~~（好像不加也行）~~：

```markdown
layout: page
```

以下设置不适用NexT主题，因此划掉。

~~如果设置为`layout: tagcloud`则为标签云页面。~~

~~如果设置为`layout: timeline`则为时间线页面，具体在`theme`的`_config.yml`设置。~~

~~如果设置为`layout: single-column`则为单栏页面。~~

### 3.8 数学公式

将`mathjax`设置为`true`即可。

```markdown
mathjax: true
```

## 4 正文书写

使用Markdown正常书写即可，参见《{% post_link markdown-basic-syntax-and-examples %}》。

**注意文中不要出现两个大括号，或大括号与井号相连等与渲染语法冲突的情况。**

```markdown
}} # Bad
} } # Good
{# # Bad
{ # # Good
```

### 4.1 摘要

文章摘要有三种提取方法，优先级从高到低：

- 提取Front-matter的`description`内容。*文字不多时用这种。*

- 在文章合适部分加上`<!--more-->`，文章会被自动截断。*一般情况下或文字较多时用这种。*

- 根据主题配置文件中的如下代码自动生成摘要。*不要用这种，观感会很差。*

  ```yaml
  auto_excerpt:
  	enable: true
  	length: 150
  ```

### 4.2 标签插件

标签插件用于在正文中以`{}`加`%`的格式，输入特定内容。

#### 4.2.1 引用块

语法如下：

```
{% blockquote [author[, source]] [link] [source_link_title] %}
content
{% endblockquote %}
```

**普通引用块**

```
{% blockquote %}
闻在宥天下，不闻治天下也。
{% endblockquote %}
```

{% blockquote %}
闻在宥天下，不闻治天下也。
{% endblockquote %}

**引用加作者、来源**

```
{% blockquote 庄子, 《庄子·外篇·在宥》 %}
闻在宥天下，不闻治天下也。
{% endblockquote %}
```

{% blockquote 庄子, 《庄子·外篇·在宥》 %}
闻在宥天下，不闻治天下也。
{% endblockquote %}

**引用自网址**

```
{% blockquote @央视新闻 https://weibo.com/2656274875/Kt4FMdzB5?from=page_1002062656274875_profile&wvr=6&mod=weibotime&type=comment#_rnd1628758139404 新浪微博 %}
8月11日0-24时，湖北省新增新冠肺炎确诊病例10例。
{% endblockquote %}
```

{% blockquote @央视新闻 https://weibo.com/2656274875/Kt4FMdzB5?from=page_1002062656274875_profile&wvr=6&mod=weibotime&type=comment#_rnd1628758139404 新浪微博 %}
8月11日0-24时，湖北省新增新冠肺炎确诊病例10例。
{% endblockquote %}

**NexT还支持居中引用**

```
{% centerquote %}
Tomorrow will be fine.
{% endcenterquote %}
```

{% centerquote %}
Tomorrow will be fine.
{% endcenterquote %}

#### 4.2.2 代码块

语法如下：

```
{% codeblock [title] [lang:language] [url] [link text] [additional options] %}
code snippet
{% endcodeblock %}
```

**普通代码块**

```
{% codeblock %}
alert('Hello World!');
{% endcodeblock %}
```

{% codeblock %}
alert('Hello World!');
{% endcodeblock %}

**指定语言**

```
{% codeblock lang:javascript %}
alert('Hello World!');
{% endcodeblock %}
```

{% codeblock lang:javascript %}
alert('Hello World!');
{% endcodeblock %}

**代码标题、网址**

```
{% codeblock time.js http://jsrun.net/new 在线运行 %}
var time = new Date();
console.log(time);
{% endcodeblock %}
```

{% codeblock time.js http://jsrun.net/new 在线运行 %}
var time = new Date();
console.log(time);
{% endcodeblock %}

#### 4.2.3 图片

语法如下：

```
{% img [class names] /path/to/image [width] [height] '"title text" "alt text"' %}
```

**固定大小的图片**

```
{% img https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/kongjianzhan.jpg 200 100 '"中国空间站" "天和核心舱"' %}
```

{% img https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/kongjianzhan.jpg 200 100 '"中国空间站" "天和核心舱"' %}

#### 4.2.4 iFrame

语法如下：

```
{% iframe url [width] [height] %}
```

**嵌入网易云音乐播放器**

```
{% iframe //music.163.com/outchain/player?type=2&id=1425676569&auto=0&height=66 330 86 %}
```

{% iframe //music.163.com/outchain/player?type=2&id=1425676569&auto=0&height=66 330 86 %}

其实直接放HTML的`<iframe>`代码也可以：

```html
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1425676569&auto=0&height=66"></iframe>
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1425676569&auto=0&height=66"></iframe>

**嵌入bilibili视频播放器**

```
{% iframe //player.bilibili.com/player.html?aid=92523122&bvid=BV1eE411H7XL&cid=157973222&page=1 %}
```

{% iframe //player.bilibili.com/player.html?aid=92523122&bvid=BV1eE411H7XL&cid=157973222&page=1 %}

#### 4.2.5 引用文章

引用其他文章的链接，语法为：

```
{% post_path filename %}
{% post_link filename [title] [escape] %}
```

**链接使用文章标题**

```
{% post_link build-corecabin-in-one-week %}
```

{% post_link build-corecabin-in-one-week %}

**链接使用自定义文字**

```
{% post_link build-corecabin-in-one-week '点击进入文章'%}
```

{% post_link build-corecabin-in-one-week '点击进入文章'%}

**标题特殊字符转义、禁止转义（false）**

```
{% post_link hexo-4-released 'How to use <b> tag in title' %}
{% post_link hexo-4-released '<b>bold</b> custom title' false %}
```

示例略

#### 4.2.6 Bootstrap Callout（NexT主题）

语法如下：

```
{% note class_name %} Content (md partial supported) {% endnote %}
```

`class_name`可以设置为`default`、`primary`、`success`、`info`、`warning`或`danger`。

```
{% note defalut %} **defalut** {% endnote %}
{% note primary %} **primary** {% endnote %}
{% note success %} **success** {% endnote %}
{% note info %} **info** {% endnote %}
{% note warning %} **warning** {% endnote %}
{% note danger %} **danger** {% endnote %}
```

{% note defalut %} **defalut** {% endnote %}
{% note primary %} **primary** {% endnote %}
{% note success %} **success** {% endnote %}
{% note info %} **info** {% endnote %}
{% note warning %} **warning** {% endnote %}
{% note danger %} **danger** {% endnote %}

## 5 发布

根目录下依次执行hexo三连：

```bash
$ hexo generate #生成静态html到public
$ hexo server #生成预览窗口（localhost:4000，ctrl+C停止）
$ hexo deploy #部署到Github（上传public代码到main分支）
```

也可以缩写成

```bash
$ hexo g
$ hexo s
$ hexo d
```

大功告成！

## 6 备份代码（多端）

根目录下依次执行：

```bash
$ git add .
$ git commit -m "comments"
$ git push origin hexo
```

以更新远程仓库代码。

这个操作也可以通过IDE或[GitHub Desktop](https://desktop.github.com/)进行，而且更推荐！

## 7 多端管理说明

~~考虑到适用GitHub托管hexo源码存在或多或少的问题，尤其是`git pull`和`git push`与GitHub连接的加载速度太慢（因为办公室非Chrome时科学上网比较困难），所以可以使用如下方法：~~

- ~~当临时出差时：~~

  ~~用Markdown写作，回来再`new post`。~~

- ~~当长期不在时：~~

  ~~将整个`hexo`用U盘或其他方式拷贝到笔记本上（反正也才几十Mb），然后在笔记本已安装hexo、设置好GitHub SSH密钥的情况下，执行`npm install`即可。此时可以通过`hexo s`查看是否成功。~~

- ~~移动端时：~~

  ~~用Simplenote写Markdown，回来再`new post`。好像也有其他方法，但暂时不用。~~

目前核心舱的资源和代码已经分别进行远程管理，资源通过坚果云同步，代码通过GitHub同步，这是目前在多台电脑管理hexo比较合适的方式。

因此多端管理hexo时，仅需同步GitHub仓库的hexo分支即可，资源通过坚果云自动同步，静态页面通过`hexo d`部署。移动端通过坚果云或语雀临时存储文稿就行。
