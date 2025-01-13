---
title: Markdown 基本语法与示例
date: 2021-08-04 00:18:05
categories: Markdown
tags:
  - Markdown
  - HTML
  - 技术
comments: true
mathjax: true
---

Markdown 是一种纯文本格式标记语言，它通过简单的标记语法实现了格式排版，且易学、易读、易写，因此非常流行。本文及本博客的文章均使用 Markdown 语法书写。

<!--more-->

## 0 简单介绍

**优点：**

1. 纯文本，编辑效果一致，可以无视排版，专心写作。
2. 操作简单，例如新增标题只需要在标题内容前加 `#` 即可。
3. 与 HTML 高度契合，可以直接使用原生 HTML 标签。

**缺点：**

1. 需要记一些语法，但非常简单。
2. 有些平台不支持 Markdown 编辑模式。
3. 对于富文本支持不好，例如无法设置不同大小的文字（但可通过 CSS 样式修改）。

**常用编辑器：**

- Typora（Windows & MacOS，大力推荐，与其他编辑器不同的是编辑效果即排版效果，无需左右分栏对照）
- VS Code（Windows & MacOS，可以结合相关插件实现更好的编辑和展示效果）
- Simplenote（iOS，免费，支持中文、云端存储、iPad）
- 
## 1 标题

标题文字前面加 `#` 即可，注意 `#` 与文字之间要有空格。几级标题加几个 `#`，共有六级标题。例如本段落标题的源码为：

```markdown
## 1、标题
```

## 2 字体

### 2.1 粗体

使用 `**` 或 `__` 包在文字两端。例如：`**粗体**` 效果为 **粗体**。

### 2.2 斜体

使用 `*` 或 `_` 包在文字两端。例如：`*斜体*` 效果为 *斜体*。

当然，粗体斜体可以嵌套使用，例如：`***粗体+斜体***` 效果为 ***粗体+斜体***。

### 2.3 高亮

~~使用 `==` 包在文字两端。例如：`==高亮==` 效果为 ==高亮==。~~

Typora 需要设置开启高亮。并且如你所见，核心舱不支持高亮。

但我们可以使用 HTML 的 `mark` 标签来实现高亮。例如：`<mark>高亮</mark>` 效果为 <mark>高亮</mark>。

### 2.4 删除线

使用 `~~` 包在文字两端。例如：`~~删除线~~` 效果为 ~~删除线~~。

## 3 引用

引用文字前面加 `>` 即可。引用可以嵌套，使用 `>>`、`>>>`……以此类推。

例如：

```markdown
> 一级引用
>> 二级引用
>>>>> 五级引用
> > >
> 回退到三级引用
>
> 回退到一级引用
```

效果为

> 一级引用
>
> > 二级引用
> >
> > >>> 五级引用
> > >
> > >回退到三级引用
>
> 回退到一级引用

## 4 分割线

三个或三个以上的 `-` 或 `*`（Typora 还支持 `+`）。

例如：

```markdown
---
-----
***
*****
```

效果为

---

-----

***

*****

（一毛一样）

## 5 HTML

直接输入就行，会自动渲染。例如：`<b>HTML 加粗</b>` 效果为 <b>HTML 加粗</b>。再例如：

```html
<font face="黑体" size="5" color="red">HTML 内容</font>
```

效果为

<font face="黑体" size="5" color="red">HTML 内容</font>

甚至可以来段更复杂的：

```html
<form>
用户名: <input type="text" name="user"><br>
密　码: <input type="password" name="pwd"><br>
性　别: <input type="radio" name="sex" value="男" checked>男&nbsp;
    <input type="radio" name="sex" value="女">女<br>
<button type="button" name="Submit" Onclick="alert('成功')">提交</button>
<button type="reset" name="Reset">重置</button>
</form>
```

效果为

<form>
用户名: <input type="text" name="user"><br>
密　码: <input type="password" name="pwd"><br>
性　别: <input type="radio" name="sex" value="男" checked>男&nbsp;
    <input type="radio" name="sex" value="女">女<br>
<button type="button" name="Submit" Onclick="alert('成功')">提交</button>
<button type="reset" name="Reset">重置</button>
</form>

（点一下试试）

## 6 超链接

使用 `[name](link "tooltip")` 表示超链接，其中 `name` 是超链接文字，`link` 是超链接地址，`tooltip` 是悬浮文字提示（可选）。

例如：

```markdown
[卡洛的核心舱](https://corecabin.cn "你好")
```

效果为

[卡洛的核心舱](https://corecabin.cn "你好")

还可以用 HTML 的 `a` 标签，例如：

```html
<a href="https://corecabin.cn" target="_blank">卡洛的核心舱</a>
```

效果为

<a href="https://corecabin.cn" target="_blank">卡洛的核心舱</a>

其中 `target="_blank"` 表示在新标签打开。

当遇到纯链接时，可以用 `<` 和 `>` 将其包起来，防止后面的文字被解析，例如：

```markdown
<https://corecabin.cn/>这是包起来的效果
https://corecabin.cn/这是不包起来的效果
```

<https://corecabin.cn/>这是包起来的效果
https://corecabin.cn/这是不包起来的效果

## 7 图片

使用 `![text](link "tooltip")` 表示图片，其中 `text` 是显示在图片下方的说明，`link` 是图片路径，可以用相对路径、绝对路径、互联网路径，`tooltip` 是悬浮文字提示（可选）。

例如（互联网路径）：

```markdown
![中国空间站](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/kongjianzhan.jpg "天和核心舱")
```

效果为

![中国空间站](https://cdn.jsdelivr.net/gh/kaluojushi/Corecabin-Picbed/img/kongjianzhan.jpg "天和核心舱")

以及套娃设置图片的超链接，例如（相对路径）：

```markdown
[![卡洛的核心舱](/img/corecabin-textlogo-bgcolor.jpg)](https://corecabin.cn)
```

效果为：

[![卡洛的核心舱](/img/corecabin-textlogo-bgcolor.jpg)](https://corecabin.cn)

注意以上情况在当前主题下可能会遇到图片文字说明的显示 bug。

## 8 列表

### 8.1 无序列表

文字前面加 `-` 或 `*` 或 `+` 即可，注意符号与文字之间要有空格。

例如：

```markdown
- 无序列表 1
* 无序列表 2
+ 无序列表 3
```

效果为

- 无序列表 1

* 无序列表 2

+ 无序列表 3

### 8.2 有序列表

文字前面加数字与 `.` 即可，注意符号与文字之间要有空格。注意显示效果与实际输入数字无关，而与当前次序有关。

例如：

```markdown
1. 第一点
2. 第二点
4. 大概是第四点？
```

效果为

1. 第一点
2. 第二点
4. 大概是第四点？

### 8.3 列表嵌套

不同级列表之间通过缩进即可嵌套，例如：

```markdown
- 一级无序
	+ 二级无序 1
	+ 二级无序 2
1. 一级有序
	1. 二级有序
		+ 三级无序
```

效果为

- 一级无序
  + 二级无序 1
  + 二级无序 2

1. 一级有序
   1. 二级有序
      + 三级无序

## 9 代码

### 9.1 行内代码

使用 <code>`</code> 包在代码两端。例如：

```markdown
`javascript`
```

效果为

`javascript`

本文也大量用到行内代码引用文字。

### 9.2 代码块

使用 3 个 <code>`</code> 包在代码两端，第一行可以加入代码语言，例如：

````markdown
```javascript
function fact(num) {
  if (num === 1) {
    return 1;
  }
  else {
    return num * fact(num - 1);
  }
}
```
````

效果为

```javascript
function fact(num) {
  if (num === 1) {
    return 1;
  }
  else {
    return num * fact(num - 1);
  }
}
```

本文也大量用到代码块表示 Markdown 示例。

需要在代码块中使用代码块时，外层代码块可以使用 4 个 <code>`</code>。

## 10 数学公式

Markdown 支持 LaTeX 公式。行内公式用 ` $ ` 包在公式两端（Typora 需要设置开启），公式块用 ` $$ ` 包在公式两端，例如：

```latex
当 $a \ne 0$，此方程式有两个解 $ax^2 + bx + c = 0$，他们是
$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}.
$$
```

效果为

当 $a \ne 0$，此方程式有两个解 $ax^2 + bx + c = 0$，他们是
$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}.
$$

更多公式相关见《{% post_link latex-mathematical-formula-handbook %}》。

## 11 任务列表（Todo List）

文字前面加 `- [ ] ` 或 `- [x] ` 即可，空格表示未完成，`x` 表示已完成。

例如：

```markdown
- [x] 甜甜圈
- [x] 珍珠奶茶
- [ ] 方便面
- [x] 火锅
- [ ] 米饭
- [ ] 大盘鸡
```

效果为

- [x] 甜甜圈
- [x] 珍珠奶茶
- [ ] 方便面
- [x] 火锅
- [ ] 米饭
- [ ] 大盘鸡

## 12 表格

语法为：

```markdown
表头|表头|表头
---|---|---
内容|内容|内容
内容|内容|内容
```

第二行用来分割表头与内容。`-` 的个数有一个就行，此处多加了几个。

文字默认居左。`-` 的两边加 `:` 表示该列居中，`-` 的右边加 `:` 表示该列居右。

例如：

```markdown
姓名（居左）|年级（居中）|年龄（居右）
-----|:-----:|-----:
张三|大三|21
李四|大一|18
王五|研二|24
```

效果为

| 姓名（居左） | 年级（居中） | 年龄（居右） |
| ------------ | :----------: | -----------: |
| 张三         |     大三     |           21 |
| 李四         |     大一     |           18 |
| 王五         |     研二     |           24 |

## 13 转义字符

在输入特殊符号，尤其是 Markdown 和 HTML 中的部分符号时，可以使用转义字符。

### 13.1 Markdown 转义字符

使用 `\` 置于符号前。

例如输入 `*你好*` 会显示 *你好*，输入 `\*你好\*` 效果为 \*你好\*。

具体如下：

| 字符 |    含义    |       转义字符       |
| :--: | :--------: |:----------------:|
|  \   |   反斜杠   |       `\\`       |
|<span>`</span>|  反引号 | <code>\\`</code> |
|  *   |    星号    |       `\*`       |
|  _   |   下划线   |       `\_`       |
|  {}  |   花括号   |      `\{\}`      |
|  []  |   方括号   |      `\[\]`      |
|  ()  |   小括号   |      `\(\)`      |
|  #   |    井号    |       `\#`       |
|  +   |    加号    |       `\+`       |
|  -   |    减号    |       `\-`       |
|  .   |  英文句点  |       `\.`       |
|  !   | 英文感叹号 |       `\!`       |

### 13.2 HTML 转义字符

直接输入 HTML 转义字符，具体如下：

| 显示结果 |   描述   |        输入         | 实体编号 |
| :------: | :------: |:-----------------:| :------: |
|          |   空格   |     `&nbsp;`      | `&#160;` |
|    <     |  小于号  |      `&lt;`       | `&#60;`  |
|    >     |  大于号  |      `&gt;`       | `&#62;`  |
|    &     |   和号   |      `&amp;`      | `&#38;`  |
|    "     |   引号   |     `&quot;`      | `&#34;`  |
|    '     |   撇号   | `&apos; `(IE 不支持) | `&#39;`  |
|    ￠    |    分    |     `&cent;`      | `&#162;` |
|    £     |    镑    |     `&pound;`     | `&#163;` |
|    ¥     |   日圆   |      `&yen;`      | `&#165;` |
|    §     |    节    |     `&sect;`      | `&#167;` |
|    ©     |   版权   |     `&copy;`      | `&#169;` |
|    ®     | 注册商标 |      `&reg;`      | `&#174;` |
|    ×     |   乘号   |     `&times;`     | `&#215;` |
|    ÷     |   除号   |    `&divide;`     | `&#247;` |

