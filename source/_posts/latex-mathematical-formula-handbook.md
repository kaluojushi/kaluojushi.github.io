---
title: LaTeX数学公式手册
date: 2021-08-11 20:07:35
categories: LaTeX
tags:
  - LaTeX
  - Markdown
  - 技术
  - 技巧
  - 数学
comments: true
mathjax: true
---

LaTeX是一种基于TeX的排版系统。由于其对复杂的数学公式排版效果很好，所以常用于大型论文排版和数学公式输入。本文及本博客的文章均使用LaTeX输入数学公式。

<!--more-->

## 0 简单介绍

**LaTeX**

标准写法为$ L^AT_EX $​，读音为“拉泰赫”。它是一种基于TeX的排版系统，对于生成复杂表格和数学公式表现尤为突出，适用于论文写作、数学科研类PPT制作等。

**MathJax**

它是一个JavaScript引擎，用来显示网络上的数学公式。本博客的公式使用MathJax引擎渲染。本质上，Typora的公式也是用MathJax渲染的。

**本文的公式显示环境**

- Markdown
- LaTeX
- MathJax v3.0.0
- hexo-renderer-kramed渲染器

**注意**

应特别注意Markdown渲染为HTML及其与LaTeX语法的冲突，这会影响文章效果。如应避免大括号重复出现，应加上空格。

```latex
}} % Bad
} } % Good
```

## 1 公式插入

公式分为行内公式和公式块，前者嵌入行内，后者单独成行。

行内公式表示方法为：` $ 公式 $ `，公式块表示方法为：` $$ 公式 $$ `，公式中的空格会被忽略。公式块可通过`\tag{n}`进行手动编号。

**行内公式**	

```latex
$ f(x) = a+b $
```

$ f(x) = a+b $​​​​​

**公式块**

注意Typora中建议写成以下形式，否则仍会显示为行内公式：

```latex
$$
x+2 = 3*4
$$
```

$$
x+2 = 3*4
$$

除强调区分行内公式与公式块外，下文的公式示例均省略`$`符号。

**手动编号**

```latex
x+2 = 3*4 \tag{1.1}
```

$$
x+2 = 3*4 \tag{1.1}
$$

## 2 简单运算

### 2.1 四则运算与基本括号

拉丁字母、阿拉伯数字与`+-*/=`可以直接通过键盘输入。乘号可以用`\times`表示，除号可以用`\div`表示，点乘可以用`\cdot`表示。

```latex
a + b - c*2 + d/e = 5 \times 3 + 8 \div 4 - f \cdot g
```

$$
a + b - c*2 + d/e = 5 \times 3 + 8 \div 4 - f \cdot g
$$

小括号`()`、方括号`[]`表示其本身，花括号`{}`需要`\`转义表示。

```latex
(1+2) \quad [1+2] \quad \{1+2\}
```

$$
(1+2) \quad [1+2] \quad \{1+2\}
$$

由于LaTeX会忽略空格，因此用`\quad`表示空格，见上例。

### 2.2 基本上下标

用`_`表示下标，用`^`表示上标，并只处理一个字符，多个字符用`{}`括起来。`'`表示求导，可使用多个。

```latex
x_1 + x_{1,2}^2 = y_0 + y' + y''
```

$$
x_1 + x_{1,2}^2 = y_0 + y' + y''
$$

### 2.3 基本分式、根式

用`\frac{a}{b}`表示分式，用`\sqrt`表示平方根，用`\sqrt[n]`表示n次方根。

```latex
\frac{x}{2} + \sqrt x = \sqrt[3] {x^2+y^2}
```

$$
\frac{x}{2} + \sqrt x = \sqrt[3] {x^2+y^2}
$$

## 3 符号、函数、特殊字符

本章为符号、函数、特殊字符的输入方法，若关注语法可以直接跳过本章。

### 3.1 声调/变音/标记符号

```latex
\dot{a}, \ddot{a}, \dddot{a}, \acute{a}, \grave{a}
```

$ \dot{a}, \ddot{a}, \dddot{a}, \acute{a}, \grave{a} $

```latex
\check{a}, \breve{a}, \tilde{a}, \widetilde{a}, \bar{a}, \mathring{a}
```

$ \check{a}, \breve{a}, \tilde{a}, \widetilde{a}, \bar{a}, \mathring{a} $

```latex
\hat{a}, \widehat{a}, \vec{a}
```

$ \hat{a}, \widehat{a}, \vec{a} $

### 3.2 标准函数

**指数**

```latex
\exp_a b, \exp b, x^2
```

$ \exp_a b, \exp b, x^2 $

**对数**

```latex
\log a, \lg b, \ln c, \log_{2} d
```

$ \log a, \lg b, \ln c, \log_{2} d $

**三角函数**

```latex
\sin a, \cos b, \tan c, \cot d, \sec e, \csc f
```

$ \sin a, \cos b, \tan c, \cot d, \sec e, \csc f $

**反三角函数**

```latex
\arcsin a, \arccos b, \arctan c
```

$ \arcsin a, \arccos b, \arctan c $​​

注意`\arccot, \arcsec, \arccsc`不被转义，需要用`\operatorname`替代。

**双曲函数**

```latex
\sinh a, \cosh b, \tanh c, \coth d
```

$ \sinh a, \cosh b, \tanh c, \coth d $​

注意`\sech, \csch`不被转义，需要用`\operatorname`替代。

**绝对值**

```latex
\left\vert a \right\vert, |b|, | \dfrac cd |, \left| \dfrac cd \right|
```

$ \left\vert a \right\vert, |b|, | \dfrac cd |, \left| \dfrac cd \right| $

**最大值，最小值**

```latex
\max(x,y), \min(x,y)
```

$ \max(x,y), \min(x,y) $​​

**其他不能转义的标准函数**

用`\operatorname{function}`表示，如符号函数：

```latex
\operatorname{sgn} x
```

$ \operatorname{sgn} x $

### 3.3 界限，极限

```latex
\min x, \max y, \inf a, \sup b
```

$ \min x, \max y, \inf a, \sup b $

```latex
\lim x, \liminf y, \limsup z
```

$ \lim x, \liminf y, \limsup z $

```latex
\dim p, \deg q, \det m, \ker\phi
```

$ \dim p, \deg q, \det m, \ker\phi $​

更多极限见4.5节。

### 3.4 投射

```latex
\Pr j, \hom l, \lVert z \rVert, \arg z
```

$ \Pr j, \hom l, \lVert z \rVert, \arg z $

### 3.5 微分，导数

用`\mathrm{x}`处理非斜体字符，如微分符号`d`。

```latex
dx, \mathrm{d}x, \partial t, \nabla\psi
```

$ dx, \mathrm{d}x, \partial t, \nabla\psi $​

```latex
\mathrm{d}y/\mathrm{d}x, \frac{\mathrm{d}y}{\mathrm{d}x}, \frac{\partial^2}{\partial x \partial y}z
```

$$
\mathrm{d}y/\mathrm{d}x, \frac{\mathrm{d}y}{\mathrm{d}x}, \frac{\partial^2}{\partial x \partial y}z
$$

```latex
', \prime, \backprime, f^\prime, f', f'', f^{(3)}, \dot y, \ddot y, \dddot y
```

$ ', \prime, \backprime, f^\prime, f', f'', f^{(3)}, \dot y, \ddot y, \dddot y $

### 3.6 字母符号与常数

```latex
\infty, \aleph, \complement, \backepsilon, \eth, \Finv, \hbar
```

$ \infty, \aleph, \complement, \backepsilon, \eth, \Finv, \hbar $

```latex
\imath, \jmath, \Bbbk, \ell, \mho, \wp, \Re, \circledS
```

$ \imath, \jmath, \Bbbk, \ell, \mho, \wp, \Re, \circledS $​​​​

### 3.7 模运算

```latex
\pmod{m}, a \bmod b
```

$ \pmod{m}, a \bmod b $

```latex
\gcd(m,n), \operatorname{lcm}(m,n)
```

$ \gcd(m,n), \operatorname{lcm}(m,n) $​

```latex
\mid, \nmid, \shortmid, \nshortmid
```

$ \mid, \nmid, \shortmid, \nshortmid $

### 3.8 根号

```latex
\surd, \sqrt{2}, \sqrt[n]{}, \sqrt[3]{x^2+y^2}
```

$ \surd, \sqrt{2}, \sqrt[n]{}, \sqrt[3]{x^2+y^2} $

### 3.9 运算符

```latex
+, -, \pm, \mp, \dotplus
```

$ +, -, \pm, \mp, \dotplus $

```latex
*, /, \times, \div, \divideontimes, \cdot, \ast, \backslash
```

$ *, /, \times, \div, \divideontimes, \cdot, \ast, \backslash $

```latex
\star, \circ, \bullet
```

$ \star, \circ, \bullet $

```latex
\boxplus, \boxminus, \boxtimes, \boxdot
```

$ \boxplus, \boxminus, \boxtimes, \boxdot $

```latex
\oplus, \ominus, \otimes, \oslash, \odot
```

$ \oplus, \ominus, \otimes, \oslash, \odot $

```latex
\circleddash, \circledcirc, \circledast
```

$ \circleddash, \circledcirc, \circledast $​

```latex
\bigoplus, \bigotimes, \bigodot
```

$ \bigoplus, \bigotimes, \bigodot $

### 3.10 集合

```latex
\{, \}, \emptyset, \varnothing
```

$ \{, \}, \emptyset, \varnothing $​

注意本环境下不支持用`\O, \empty`表示空集，需要用`\emptyset`替代。​

```latex
\in, \notin, \not\in, \ni, \not\ni
```

$ \in, \notin, \not\in, \ni, \not\ni $​​

```latex
\cap, \Cap, \sqcap, \bigcap
```

$ \cap, \Cap, \sqcap, \bigcap $​

```latex
\cup, \Cup, \sqcup, \bigcup, \bigsqcup, \uplus, \biguplus
```

$ \cup, \Cup, \sqcup, \bigcup, \bigsqcup, \uplus, \biguplus $

```latex
\setminus, \smallsetminus
```

$ \setminus, \smallsetminus $

```latex
\subset, \Subset, \sqsubset
```

$ \subset, \Subset, \sqsubset $

```latex
\supset, \Supset, \sqsupset
```

$ \supset, \Supset, \sqsupset $

```latex
\subseteq, \nsubseteq, \subsetneq, \varsubsetneq, \sqsubseteq
```

$ \subseteq, \nsubseteq, \subsetneq, \varsubsetneq, \sqsubseteq $

```latex
\supseteq, \nsupseteq, \supsetneq, \varsupsetneq, \sqsupseteq
```

$ \supseteq, \nsupseteq, \supsetneq, \varsupsetneq, \sqsupseteq $

```latex
\subseteqq, \nsubseteqq, \subsetneqq, \varsubsetneqq
```

$ \subseteqq, \nsubseteqq, \subsetneqq, \varsubsetneqq $

```latex
\supseteqq, \nsupseteqq, \supsetneqq, \varsupsetneqq
```

$ \supseteqq, \nsupseteqq, \supsetneqq, \varsupsetneqq $

### 3.11 关系符号

```latex
=, \ne, \neq, \equiv, \not\equiv
```

$ =, \ne, \neq, \equiv, \not\equiv $

```latex
\doteq, \doteqdot, :=, \overset{\underset{\mathrm{def} } {} } {=}
```

$ \doteq, \doteqdot, :=, \overset{\underset{\mathrm{def} } {} } {=} $

```latex
\sim, \nsim, \backsim, \thicksim, \simeq, \backsimeq, \eqsim, \cong, \ncong
```

$ \sim, \nsim, \backsim, \thicksim, \simeq, \backsimeq, \eqsim, \cong, \ncong $

```latex
\approx, \thickapprox, \approxeq, \asymp, \propto, \varpropto
```

$ \approx, \thickapprox, \approxeq, \asymp, \propto, \varpropto $

```latex
<, \nless, \ll, \not\ll, \lll, \not\lll, \lessdot
```

$ <, \nless, \ll, \not\ll, \lll, \not\lll, \lessdot $

```latex
>, \ngtr, \gg, \not\gg, \ggg, \not\ggg, \gtrdot
```

$ >, \ngtr, \gg, \not\gg, \ggg, \not\ggg, \gtrdot $

```latex
\le, \leq, \lneq, \leqq, \nleq, \nleqq, \lneqq, \lvertneqq
```

$ \le, \leq, \lneq, \leqq, \nleq, \nleqq, \lneqq, \lvertneqq $

```latex
\ge, \geq, \gneq, \geqq, \ngeq, \ngeqq, \gneqq, \gvertneqq
```

$ \ge, \geq, \gneq, \geqq, \ngeq, \ngeqq, \gneqq, \gvertneqq $

```latex
\lessgtr, \lesseqgtr, \lesseqqgtr, \gtrless, \gtreqless, \gtreqqless
```

$ \lessgtr, \lesseqgtr, \lesseqqgtr, \gtrless, \gtreqless, \gtreqqless $

```latex
\leqslant, \nleqslant, \eqslantless
```

$ \leqslant, \nleqslant, \eqslantless $

```latex
\geqslant, \ngeqslant, \eqslantgtr
```

$ \geqslant, \ngeqslant, \eqslantgtr $

```latex
\lesssim, \lnsim, \lessapprox, \lnapprox
```

$ \lesssim, \lnsim, \lessapprox, \lnapprox $

```latex
\gtrsim, \gnsim, \gtrapprox, \gnapprox
```

$ \gtrsim, \gnsim, \gtrapprox, \gnapprox $

```latex
\prec, \nprec, \preceq, \npreceq, \precneqq
```

$ \prec, \nprec, \preceq, \npreceq, \precneqq $​

```latex
\succ, \nsucc, \succeq, \nsucceq, \succneqq
```

$ \succ, \nsucc, \succeq, \nsucceq, \succneqq $

```latex
\preccurlyeq, \curlyeqprec
```

$ \preccurlyeq, \curlyeqprec $

```latex
\succcurlyeq, \curlyeqsucc
```

$ \succcurlyeq, \curlyeqsucc $

```latex
\precsim, \precnsim, \precapprox, \precnapprox
```

$ \precsim, \precnsim, \precapprox, \precnapprox $

```latex
\succsim, \succnsim, \succapprox, \succnapprox
```

$ \succsim, \succnsim, \succapprox, \succnapprox $

### 3.12 几何符号

```latex
\parallel, \nparallel, \shortparallel, \nshortparallel
```

$ \parallel, \nparallel, \shortparallel, \nshortparallel $

```latex
\perp, \angle, \sphericalangle, \measuredangle, 45^\circ
```

$ \perp, \angle, \sphericalangle, \measuredangle, 45^\circ $

```latex
\Box, \blacksquare, \diamond, \Diamond, \lozenge, \blacklozenge, \bigstar, \bigcirc
```

$ \Box, \blacksquare, \diamond, \Diamond, \lozenge, \blacklozenge, \bigstar, \bigcirc $​

```latex
\triangle, \triangledown ,\bigtriangleup, \bigtriangledown, \vartriangle
```

$ \triangle, \triangledown ,\bigtriangleup, \bigtriangledown, \vartriangle $​

```latex
\blacktriangle, \blacktriangledown, \blacktriangleleft, \blacktriangleright
```

$ \blacktriangle, \blacktriangledown, \blacktriangleleft, \blacktriangleright $

### 3.13 逻辑符号

```latex
\forall, \not\forall, \exists, \nexists
```

$ \forall, \not\forall, \exists, \nexists $​

```latex
\because, \therefore, \And
```

$ \because, \therefore, \And $

```latex
\lor, \vee, \curlyvee, \bigvee
```

$ \lor, \vee, \curlyvee, \bigvee $​

```latex
\land, \wedge, \curlywedge, \bigwedge
```

$ \land, \wedge, \curlywedge, \bigwedge $

注意本环境下不支持用`\or, \and`表示或、且，需要用`\lor, \land`替代。

```latex
\bar{x}, \bar{abc}, \overline{x}, \overline{abc}
```

$ \bar{x}, \bar{abc}, \overline{x}, \overline{abc} $

```latex
\lnot, \neg, \not\operatorname{R}, \bot, \top
```

$ \lnot, \neg, \not\operatorname{R}, \bot, \top $

```latex
\vdash, \dashv, \vDash, \Vdash, \Vvdash, \models
```

$ \vdash, \dashv, \vDash, \Vdash, \Vvdash, \models $​​

```latex
\nvdash, \nVdash, \nvDash, \nVDash
```

$ \nvdash, \nVdash, \nvDash, \nVDash $

```latex
\ulcorner, \urcorner, \llcorner, \lrcorner
```

$ \ulcorner, \urcorner, \llcorner, \lrcorner $

### 3.14 箭头

```latex
\to, \rightarrow, \nrightarrow, \longrightarrow
```

$ \to, \rightarrow, \nrightarrow, \longrightarrow $

```latex
\gets, \leftarrow, \nleftarrow, \longleftarrow
```

$ \gets, \leftarrow, \nleftarrow, \longleftarrow $

```latex
\leftrightarrow, \nleftrightarrow, \longleftrightarrow
```

$ \leftrightarrow, \nleftrightarrow, \longleftrightarrow $

```latex
\uparrow, \downarrow, \updownarrow
```

$ \uparrow, \downarrow, \updownarrow $

```latex
\Rightarrow, \nRightarrow, \Longrightarrow, \implies
```

$ \Rightarrow, \nRightarrow, \Longrightarrow, \implies $

```latex
\Leftarrow, \nLeftarrow, \Longleftarrow
```

$ \Leftarrow, \nLeftarrow, \Longleftarrow $

```latex
\Leftrightarrow, \nLeftrightarrow, \Longleftrightarrow, \iff
```

$ \Leftrightarrow, \nLeftrightarrow, \Longleftrightarrow, \iff $

```latex
\Uparrow, \Downarrow, \Updownarrow
```

$ \Uparrow, \Downarrow, \Updownarrow $

```latex
\Rrightarrow, \Lleftarrow
```

$ \Rrightarrow, \Lleftarrow $

```latex
\nearrow, \swarrow, \nwarrow, \searrow
```

$ \nearrow, \swarrow, \nwarrow, \searrow $

```latex
\mapsto, \longmapsto
```

$ \mapsto, \longmapsto $

```latex
\rightharpoonup, \rightharpoondown, \leftharpoonup, \leftharpoondown, \upharpoonleft, \upharpoonright, \downharpoonleft, \downharpoonright, \rightleftharpoons, \leftrightharpoons
```

$ \rightharpoonup, \rightharpoondown, \leftharpoonup, \leftharpoondown, \upharpoonleft, \upharpoonright, \downharpoonleft, \downharpoonright, \rightleftharpoons, \leftrightharpoons $

```latex
\curvearrowleft, \circlearrowleft, \Lsh, \upuparrows, \rightrightarrows, \rightleftarrows, \rightarrowtail, \looparrowright
```

$ \curvearrowleft, \circlearrowleft, \Lsh, \upuparrows, \rightrightarrows, \rightleftarrows, \rightarrowtail, \looparrowright $

```latex
\curvearrowright, \circlearrowright, \Rsh, \downdownarrows, \leftleftarrows, \leftrightarrows, \leftarrowtail, \looparrowleft
```

$ \curvearrowright, \circlearrowright, \Rsh, \downdownarrows, \leftleftarrows, \leftrightarrows, \leftarrowtail, \looparrowleft $

```latex
\hookrightarrow, \hookleftarrow, \multimap, \leftrightsquigarrow, \rightsquigarrow, \twoheadrightarrow, \twoheadleftarrow
```

$ \hookrightarrow, \hookleftarrow, \multimap, \leftrightsquigarrow, \rightsquigarrow, \twoheadrightarrow, \twoheadleftarrow $

### 3.15 省略号

用`\cdots`表示居中的三个点，`\ldots`表示居底线的三个点，`\vdots`和`\ddots`分别表示垂直和对角线。

```latex
\cdots, \ldots, \vdots, \ddots
```

$ \cdots, \ldots, \vdots, \ddots $

### 3.16 其他

```latex
\amalg, \%, \&, \dagger, \ddagger
```

$ \amalg, \%, \&, \dagger, \ddagger $​​​

```latex
\smile, \frown, \wr, \triangleleft, \triangleright
```

$ \smile, \frown, \wr, \triangleleft, \triangleright $

```latex
\diamondsuit, \heartsuit, \clubsuit, \spadesuit, \Game, \flat, \natural, \sharp
```

$ \diamondsuit, \heartsuit, \clubsuit, \spadesuit, \Game, \flat, \natural, \sharp $

```latex
\diagup, \diagdown, \centerdot, \ltimes, \rtimes, \leftthreetimes, \rightthreetimes
```

$ \diagup, \diagdown, \centerdot, \ltimes, \rtimes, \leftthreetimes, \rightthreetimes $

```latex
\eqcirc, \circeq, \triangleq, \bumpeq, \Bumpeq, \doteqdot, \risingdotseq, \fallingdotseq
```

$ \eqcirc, \circeq, \triangleq, \bumpeq, \Bumpeq, \doteqdot, \risingdotseq, \fallingdotseq $

```latex
\intercal, \barwedge, \veebar, \doublebarwedge, \between, \pitchfork
```

$ \intercal, \barwedge, \veebar, \doublebarwedge, \between, \pitchfork $

```latex
\vartriangleleft, \ntriangleleft, \vartriangleright, \ntriangleright
```

$ \vartriangleleft, \ntriangleleft, \vartriangleright, \ntriangleright $

```latex
\trianglelefteq, \ntrianglelefteq, \trianglerighteq, \ntrianglerighteq
```

$ \trianglelefteq, \ntrianglelefteq, \trianglerighteq, \ntrianglerighteq $

## 4 常用数学语法

### 4.1 上下标

用`_`表示下标，用`^`表示上标，并只处理一个字符，多个字符用`{}`括起来。上下标可嵌套或同时使用。

```latex
a^2, a_2, a^{2+2}, a_{i,j}, x_2^3
```

$ a^2, a_2, a^{2+2}, a_{i,j}, x_2^3 $

前置上下标可以用空花括号`{}`承载，也可以使用`\sideset`命令。

```latex
{}_1^2X_3^4 \quad \sideset{_1^2}{_3^4} \bigotimes
```

$ {}_1^2X_3^4 \quad \sideset{_1^2}{_3^4} \bigotimes $

### 4.2 导数

撇导数用`'`或上标的`\prime`表示，注意不要漏掉上标。

```latex
x', x'', x^\prime
```

$ x', x'', x^\prime $

```latex
x\prime % Bad
```

$ x\prime % Bad $​

点导数用`\dot`、`\ddot`和`\dddot`表示。

```latex
\dot{y}, \ddot{y}, \dddot{y}
```

$ \dot{y}, \ddot{y}, \dddot{y} $

### 4.3 向量

用`\vec`、`\boldsymbol`、`\over--arrow`或`\widehat`表示。

```latex
\vec{a}, \boldsymbol{b}, \overleftarrow{ab}, \overrightarrow{cd}, \overleftrightarrow{ab}, \widehat{abc}
```

$ \vec{a}, \boldsymbol{b}, \overleftarrow{ab}, \overrightarrow{cd}, \overleftrightarrow{ab}, \widehat{abc} $​​

### 4.4 上下线

**上弧、上划线、下划线**

```latex
\overset{\frown} {AB}, \overline {abc}, \underline{def}
```

$ \overset{\frown} {AB}, \overline {abc}, \underline{def} $

**上括号**

```latex
\overbrace{1+2+\cdots+n} \quad \overbrace{1+2+\cdots+n}^{n}
```

$ \overbrace{1+2+\cdots+n} \quad \overbrace{1+2+\cdots+n}^{n} $

可以写成矩阵形式，使得上面的字符变大：

```latex
\begin{matrix} n \\ \overbrace{1+2+\cdots+n} \end{matrix}
```

$ \begin{matrix} n \\ \overbrace{1+2+\cdots+n} \end{matrix} $

**下括号**

```latex
\underbrace{a+b+\cdots+z} \quad \underbrace{a+b+\cdots+z}_{26}
```

$ \underbrace{a+b+\cdots+z} \quad \underbrace{a+b+\cdots+z}_{26} $

```latex
\begin{matrix} \underbrace{a+b+\cdots+z} \\ 26 \end{matrix}
```

$ \begin{matrix} \underbrace{a+b+\cdots+z} \\ 26 \end{matrix} $

### 4.5 大型运算符（求和求积、极限、积分等）

**注意**

大型运算符通常含有上下部分，LaTeX用上下标表示。其中行内公式位于右上右下，公式块位于正上正下（积分除外）。

```latex
$ \sum_{i=1}^{n} i^2 $
$$
\sum_{i=1}^{n} i^2
$$
```

$ \sum_{i=1}^{n} i^2 $
$$
\sum_{i=1}^{n} i^2
$$
若要在行内公式显示为正上正下，可以使用`\limits`命令跟在运算符后：

```latex
$ \sum\limits_{i=1}^{n} i^2 $
```

$ \sum\limits_{i=1}^{n} i^2 $

若要在公式块显示为右上右下，可以使用一阶无框矩阵形式或使用`\nolimits`命令：

```latex
$$
\begin{matrix} \sum_{i=1}^{n} i^2 \end{matrix}
$$
```

$$
\begin{matrix} \sum_{i=1}^{n} i^2 \end{matrix}
$$

```latex
$$
\sum\nolimits_{i=1}^{n} i^2
$$
```

$$
\sum\nolimits_{i=1}^{n} i^2
$$

下文的显示形式均为公式块。

**求和（累加）、求积（累乘）**

```latex
\sum_{i=1}^{n} i^2 \quad \prod_{i=1}^{n} x_i
```

$$
\sum_{i=1}^{n} i^2 \quad \prod_{i=1}^{n} x_i
$$

**极限**

```latex
\lim_{x \to \infty} (1+\frac{1}{x})^x = e
```

$$
\lim_{x \to \infty} (1+\frac{1}{x})^x = e
$$

**普通积分**

```latex
\int_{a}^{b} e^x \, \mathrm{d}x
```

$$
\int_{a}^{b} e^x \, \mathrm{d}x
$$

`\,`可省略，但建议加入使式子更美观；`\mathrm{d}`可替换为`{\rm d}`。

**二重积分、三重积分**

```latex
\iint_{D} f(x,y) \, \mathrm{d}\sigma \quad \iiint_{\Omega} f(x,y,z) \, \mathrm{d}V
```

$$
\iint_{D} f(x,y) \, \mathrm{d}\sigma \quad \iiint_{\Omega} f(x,y,z) \, \mathrm{d}V
$$

**闭合曲线积分**

```latex
\oint_{L} f(x,y) \, \mathrm{d}s
```

$$
\oint_{L} f(x,y) \, \mathrm{d}s
$$

**其他积分符号**

```latex
\int, \iint, \iiint, \iiiint, \idotsint, \oint
```

$ \int, \iint, \iiint, \iiiint, \idotsint, \oint $​​

注意本环境下不支持用`\oiint, \oiiint`表示二重闭合积分、三重闭合积分。

**交集、并集、余积**

```latex
\bigcap_{i=1}^{n} A_i \quad \bigcup_{j=1}^{m} B_j
```

$$
\bigcap_{i=1}^{n} A_i \quad \bigcup_{j=1}^{m} B_j
$$

```latex
\coprod_{i \in I} A_i
```

$$
\coprod_{i \in I} A_i
$$

## 5 分式

### 5.1 基本输入

分式可以通过`\over`命令，两侧标记分子分母，且整体需要用花括号括起来。

```latex
x = { {-b \pm \sqrt{b^2-4ac} } \over {2a} }
```

$$
x = { {-b \pm \sqrt{b^2-4ac} } \over {2a} }
$$
常用的分式用`\frac{分子}{分母}`命令，便捷时使用`\frac ab`快速生成$ \frac ab $​。

```latex
\frac{-b \pm \sqrt{b^2-4ac} }{2a} \quad \frac ab
```

$$
\frac{-b \pm \sqrt{b^2-4ac} }{2a} \quad \frac ab
$$
### 5.2 分式排版

普通分式在行内公式会自动缩小，在公式块会显示为完整大小。

```latex
$ \frac{1}{2}=0.5 $
$$
\frac{1}{2}=0.5
$$
```

$ \frac{1}{2}=0.5 $​
$$
\frac{1}{2}=0.5
$$
`\tfrac`命令用于使分式显示为*行内公式样式*。

```latex
$$
\tfrac{1}{2}=0.5
$$
```

$$
\tfrac{1}{2}=0.5
$$

`\dfrac`命令用于使分式显示为*公式块样式*。

```latex
$ \dfrac{1}{2}=0.5 $
```

$ \dfrac{1}{2}=0.5 $​

**注意**

在指数函数、极限、积分等场景下，尽量不使用`\frac`命令，而使用`/`表示为横式分式。

```latex
$$
e^{\frac{i\pi}{2} } \quad \int_{-\frac{\pi}{2} }^{\frac{\pi}{2} } \sin x \, \mathrm{d}x
% Bad
$$
```

$$
e^{\frac{i\pi}{2} } \quad \int_{-\frac{\pi}{2} }^{\frac{\pi}{2} } \sin x \, \mathrm{d}x
% Bad
$$
```latex
$$
e^{i\pi/2} \quad \int_{-\pi/2}^{\pi/2} \sin x \, \mathrm{d}x
% Good
$$
```

$$
e^{i\pi/2} \quad \int_{-\pi/2}^{\pi/2} \sin x \, \mathrm{d}x
% Good
$$

### 5.3 连分式

用`\cfrac`命令输入连分式，会自动处理分子分母的高度。

```latex
$$
\cfrac{1}{2 + \cfrac{1}{2 + \cfrac{1}{2 + \cdots} } }
$$
```

$$
\cfrac{1}{2 + \cfrac{1}{2 + \cfrac{1}{2 + \cdots} } }
$$
###  5.4 二项式系数

用`\binom`命令输入，`\tbinom`使二项式系数显示为行内公式样式，`\dbinom`使二项式系数显示为公式块样式。

```latex
$$
\mathrm{C}_n^r = \binom{n}{r}
$$
```

$$
\mathrm{C}_n^r = \binom{n}{r}
$$

```latex
$$
\tbinom{n}{r} = \tbinom{n}{n-r}
$$
```

$$
\tbinom{n}{r} = \tbinom{n}{n-r}
$$

```latex
$ \dbinom{n}{r} = \dfrac{n!}{k!\,(n-k)!} $
```

$ \dbinom{n}{r} = \dfrac{n!}{k!\,(n-k)!} $​

## 6 矩阵、条件表达式、方程组

语法为：

```latex
\begin{类型}
内容
\end{类型}
```

类型可以是：矩阵`matrix`、`pmatrix`、`bmatrix`、`Bmatrix`、`vmatrix`、`Vmatrix`，条件表达式`cases`，多行对齐方程式`aligned`、`alignedat`。

内容中，`&`符号表示每行的对齐内容，`\\`表示结尾处换行。

### 6.1 无框矩阵

用`&`分隔矩阵列，用`\\`分隔矩阵行。

```latex
\begin{matrix}
a & b \\
c & d
\end{matrix}
```

$$
\begin{matrix}
a & b \\
c & d
\end{matrix}
$$

### 6.2 有框矩阵

`pmatrix`为圆括号，`bmatrix`为方括号，`Bmatrix`为花括号，`vmatrix`为竖线（行列式），`Vmatrix`为双竖线。

使用`\cdots`$\cdots$、`\ddots`$\ddots$、`\vdots`$\vdots$输入省略号。

```latex
\begin{pmatrix}
0 & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & 0 \\
\end{pmatrix}
```

$$
\begin{pmatrix}
0 & \cdots & 0 \\
\vdots & \ddots & \vdots \\
0 & \cdots & 0 \\
\end{pmatrix}
$$

```latex
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
\quad
\begin{Bmatrix}
a & b \\
c & d
\end{Bmatrix}
\quad
\begin{vmatrix}
a & b \\
c & d
\end{vmatrix}
\quad
\begin{Vmatrix}
a & b \\
c & d
\end{Vmatrix}
```

$$
\begin{bmatrix}
a & b \\
c & d
\end{bmatrix}
\quad
\begin{Bmatrix}
a & b \\
c & d
\end{Bmatrix}
\quad
\begin{vmatrix}
a & b \\
c & d
\end{vmatrix}
\quad
\begin{Vmatrix}
a & b \\
c & d
\end{Vmatrix}
$$

### 6.3 条件表达式

用`&`分隔公式与条件。

```latex
f(n)=
\begin{cases}
n/2, & \text{if } n \text{ is even} \\
3n+1, & \text{if } n \text{ is odd}
\end{cases}
```

$$
f(n)=
\begin{cases}
n/2, & \text{if } n \text{ is even} \\
3n+1, & \text{if } n \text{ is odd}
\end{cases}
$$

`\text`表示字符为文本格式，而非数学格式，注意空格处理。

### 6.4 多行等式、同余式

```latex
\begin{aligned}
f(x) & = (m+n)^2 \\
& = m^2+2mn+n^2 \\
\end{aligned}
```

$$
\begin{aligned}
f(x) & = (m+n)^2 \\
& = m^2+2mn+n^2 \\
\end{aligned}
$$

```latex
\begin{aligned}
3^{6n+3}+4^{6n+3}
& \equiv (3^3)^{2n+1}+(4^3)^{2n+1} \\
& \equiv 27^{2n+1}+64^{2n+1} \\
& \equiv 27^{2n+1}+(-27)^{2n+1} \\
& \equiv 27^{2n+1}-27^{2n+1} \\
& \equiv 0 \pmod{91} \\
\end{aligned}
```

$$
\begin{aligned}
3^{6n+3}+4^{6n+3}
& \equiv (3^3)^{2n+1}+(4^3)^{2n+1} \\
& \equiv 27^{2n+1}+64^{2n+1} \\
& \equiv 27^{2n+1}+(-27)^{2n+1} \\
& \equiv 27^{2n+1}-27^{2n+1} \\
& \equiv 0 \pmod{91} \\
\end{aligned}
$$

`\alignedat`用于确定行数的对齐。

```latex
\begin{alignedat}{3}
f(x) & = (m+n)^2 \\
g(x) & = (m-n)^2 \\
& = m^2-2mn+n^2 \\
\end{alignedat}
```

$$
\begin{alignedat}{3}
f(x) & = (m+n)^2 \\
g(x) & = (m-n)^2 \\
& = m^2-2mn+n^2 \\
\end{alignedat}
$$

### 6.5 方程组

**用`cases`表达**

```latex
\begin{cases}
3x+5y+z=0 \\
7x-2y+4z=0 \\
-6x+3y+2z=0 \\
\end{cases}
```

$$
\begin{cases}
3x+5y+z=0 \\
7x-2y+4z=0 \\
-6x+3y+2z=0 \\
\end{cases}
$$

**用`aligned`表达**

```latex
\left\{ \begin{aligned}
3x+5y+z=0 \\
7x-2y+4z=0 \\
-6x+3y+2z=0 \\
\end{aligned} \right.
```

$$
\left\{ \begin{aligned}
3x+5y+z=0 \\
7x-2y+4z=0 \\
-6x+3y+2z=0 \\
\end{aligned} \right.
$$

## 7 数组与表格

### 7.1 基本数组与表格

数组和表格以`\begin{array}{定义式}`开头，以`\end{array}`结尾。定义式中定义每列对齐方式，可用`l`、`c`、`r`分别代表居左、居中、居右。若插入水平分割线，在行内容间插入`\hline`；若插入垂直分割线，在定义式中插入`|`。表格内容用`&`分隔列，用`\\`分隔行。

```latex
\begin{array}{c|lcr}
x & 1\text{(left)} & 2\text{(center)} & 3\text{(right)} \\
\hline
f(x) & 0 & 10 & 55.5 \\
g(x) & -1 & 100 & 9 \\
h(x) & 2i & 1000 & 1+10i
\end{array}
```

$$
\begin{array}{c|lcr}
x & 1\text{(left)} & 2\text{(center)} & 3\text{(right)} \\
\hline
f(x) & 0 & 10 & 55.5 \\
g(x) & -1 & 100 & 9 \\
h(x) & 2i & 1000 & 1+10i
\end{array}
$$

### 7.2 用数组与表格排版

可以用数组与表格实现类似`aligned`的功能。

```latex
\begin{array}{lcr}
z & = & a \\
f(x,y,z) & = & x+y+z
\end{array}
```

$$
\begin{array}{lcr}
z & = & a \\
f(x,y,z) & = & x+y+z
\end{array}
$$

```latex
\begin{array}{cccc}
x & y & x \lor y & x \land y \\
\hline
0 & 0 & 0 & 0 \\
1 & 0 & 1 & 0 \\
0 & 1 & 1 & 0 \\
1 & 1 & 1 & 1
\end{array}
```

$$
\begin{array}{cccc}
x & y & x \lor y & x \land y \\
\hline
0 & 0 & 0 & 0 \\
1 & 0 & 1 & 0 \\
0 & 1 & 1 & 0 \\
1 & 1 & 1 & 1
\end{array}
$$

### 7.3 嵌套数组与表格

```latex
% outer
\begin{array}{c}
	% inner row 1
	\begin{array}{cc}
		% inner row 1 column 1
		\begin{array}{c|cccc}
		\min & 0 & 1 & 2 & 3 \\
		\hline
		0 & 0 & 0 & 0 & 0 \\
		1 & 0 & 1 & 1 & 1 \\
		2 & 0 & 1 & 2 & 2 \\
		3 & 0 & 1 & 2 & 3
		\end{array}
	&
		% inner row 1 column 2
		\begin{array}{c|cccc}
		\max & 0 & 1 & 2 & 3 \\
		\hline
		0 & 0 & 1 & 2 & 3 \\
		1 & 1 & 1 & 2 & 3 \\
		2 & 2 & 2 & 2 & 3 \\
		3 & 3 & 3 & 3 & 3
		\end{array}
	\end{array}
\\
	% inner row 2
	\begin{array}{c|cccc}
	\Delta & 0 & 1 & 2 & 3 \\
	\hline
	0 & 0 & 1 & 2 & 3 \\
	1 & 1 & 0 & 1 & 2 \\
	2 & 2 & 1 & 0 & 1 \\
	3 & 3 & 2 & 1 & 0
	\end{array}
\end{array}
```

$$
% outer
\begin{array}{c}
	% inner row 1
	\begin{array}{cc}
		% inner row 1 column 1
		\begin{array}{c|cccc}
		\min & 0 & 1 & 2 & 3 \\
		\hline
		0 & 0 & 0 & 0 & 0 \\
		1 & 0 & 1 & 1 & 1 \\
		2 & 0 & 1 & 2 & 2 \\
		3 & 0 & 1 & 2 & 3
		\end{array}
	&
		% inner row 1 column 2
		\begin{array}{c|cccc}
		\max & 0 & 1 & 2 & 3 \\
		\hline
		0 & 0 & 1 & 2 & 3 \\
		1 & 1 & 1 & 2 & 3 \\
		2 & 2 & 2 & 2 & 3 \\
		3 & 3 & 3 & 3 & 3
		\end{array}
	\end{array}
\\
	% inner row 2
	\begin{array}{c|cccc}
	\Delta & 0 & 1 & 2 & 3 \\
	\hline
	0 & 0 & 1 & 2 & 3 \\
	1 & 1 & 0 & 1 & 2 \\
	2 & 2 & 1 & 0 & 1 \\
	3 & 3 & 2 & 1 & 0
	\end{array}
\end{array}
$$

### 7.4 分割矩阵

在需要分割处的定义式加入`|`。

```latex
\left[
\begin{array}{cc|c}
1 & 2 & 3 \\
4 & 5 & 6
\end{array}
\right]
```

$$
\left[
\begin{array}{cc|c}
1 & 2 & 3 \\
4 & 5 & 6
\end{array}
\right]
$$

## 8 字体

普通字符可以通过`\large`、`\small`控制大小。

```latex
A, \large{A}, \small{A}
```

$ A, \large{A}, \small{A} $

### 8.1 希腊字母

输入`\`加其字母名称即可，大写字母将名称首字母大写。注意本环境下部分大写希腊字母不支持转义，需要用相似的*拉丁字母*替代。

```latex
\begin{array}{c|l|c|l}
A \alpha & \verb|A \alpha| & N \nu & \verb|N \nu| \\
\hline
B \beta & \verb|B \beta| & \Xi \xi & \verb|\Xi \xi| \\
\hline
\Gamma \gamma & \verb|\Gamma \gamma| & O \omicron & \verb|O \omicron| \\
\hline
\Delta \delta & \verb|\Delta \delta| & \Pi \pi & \verb|\Pi \pi| \\
\hline
E \epsilon & \verb|E \epsilon| & P \rho & \verb|P \rho| \\
\hline
Z \zeta & \verb|Z \zeta| & \Sigma \sigma & \verb|\Sigma \sigma| \\
\hline
H \eta & \verb|H \eta| & T \tau & \verb|T \tau| \\
\hline
\Theta \theta & \verb|\Theta \theta| & \Upsilon \upsilon & \verb|\Upsilon \upsilon| \\
\hline
I \iota & \verb|I \iota| & \Phi \phi & \verb|\Phi \phi| \\
\hline
K \kappa & \verb|K \kappa| & X \chi & \verb|X \chi| \\
\hline
\Lambda \lambda & \verb|\Lambda \lambda| & \Psi \psi & \verb|\Psi \psi| \\
\hline
M \mu & \verb|M \mu| & \Omega \omega & \verb|\Omega \omega| \\
\end{array}
```

$$
\begin{array}{c|l|c|l}
A \alpha & \verb|A \alpha| & N \nu & \verb|N \nu| \\
\hline
B \beta & \verb|B \beta| & \Xi \xi & \verb|\Xi \xi| \\
\hline
\Gamma \gamma & \verb|\Gamma \gamma| & O \omicron & \verb|O \omicron| \\
\hline
\Delta \delta & \verb|\Delta \delta| & \Pi \pi & \verb|\Pi \pi| \\
\hline
E \epsilon & \verb|E \epsilon| & P \rho & \verb|P \rho| \\
\hline
Z \zeta & \verb|Z \zeta| & \Sigma \sigma & \verb|\Sigma \sigma| \\
\hline
H \eta & \verb|H \eta| & T \tau & \verb|T \tau| \\
\hline
\Theta \theta & \verb|\Theta \theta| & \Upsilon \upsilon & \verb|\Upsilon \upsilon| \\
\hline
I \iota & \verb|I \iota| & \Phi \phi & \verb|\Phi \phi| \\
\hline
K \kappa & \verb|K \kappa| & X \chi & \verb|X \chi| \\
\hline
\Lambda \lambda & \verb|\Lambda \lambda| & \Psi \psi & \verb|\Psi \psi| \\
\hline
M \mu & \verb|M \mu| & \Omega \omega & \verb|\Omega \omega| \\
\end{array}
$$

伽玛函数可以用`digamma`表示，另外部分变量形式可以用`\var-`开头。

```latex
\digamma, \varepsilon, \vartheta, \varkappa, \varpi, \varrho, \varsigma, \varphi
```

$ \digamma, \varepsilon, \vartheta, \varkappa, \varpi, \varrho, \varsigma, \varphi $

### 8.2 希伯来符号

```latex
\aleph, \beth, \gimel, \daleth
```

$ \aleph, \beth, \gimel, \daleth $

### 8.3 特殊字体形式

#### 8.3.1 黑板报粗体

用`\mathbb{text}`或`\Bbb{text}`表示。

```latex
\begin{array}{c|ccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{mathbb} & \mathbb{SAMPLE} & \mathbb{sample} & \mathbb{0123} \\
\hline
\text{Bbb} & \Bbb{SAMPLE} & \Bbb{sample} & \Bbb{0123}
\end{array}
```

$$
\begin{array}{c|ccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{mathbb} & \mathbb{SAMPLE} & \mathbb{sample} & \mathbb{0123} \\
\hline
\text{Bbb} & \Bbb{SAMPLE} & \Bbb{sample} & \Bbb{0123}
\end{array}
$$

#### 8.3.2 粗体

用`\mathbf{text}`或`{\bf text}`表示，注意控制范围，对特殊符号无效。

```latex
\begin{array}{c|ccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{mathbf} & \mathbf{SAMPLE} & \mathbf{sample} & \mathbf{0123} \\
\hline
\text{bf} & {\bf SAMPLE} & {\bf sample} & {\bf 0123}
\end{array}
```

$$
\begin{array}{c|ccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{mathbf} & \mathbf{SAMPLE} & \mathbf{sample} & \mathbf{0123} \\
\hline
\text{bf} & {\bf SAMPLE} & {\bf sample} & {\bf 0123}
\end{array}
$$

#### 8.3.3 粗体符号

用`\boldsymbol{text}`表示，对特殊符号有效。

```latex
\begin{array}{c|cccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega & \sin\in\to \\
\hline
\text{blodsymbol} & \boldsymbol{SAMPLE} & \boldsymbol{sample} & \boldsymbol{0123} & \boldsymbol{\delta\theta\sigma\omega} & \boldsymbol{\Delta\Theta\Sigma\Omega} & \boldsymbol{\sin\in\to} \\
\end{array}
```

$$
\begin{array}{c|cccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega & \sin\in\to \\
\hline
\text{blodsymbol} & \boldsymbol{SAMPLE} & \boldsymbol{sample} & \boldsymbol{0123} & \boldsymbol{\delta\theta\sigma\omega} & \boldsymbol{\Delta\Theta\Sigma\Omega} & \boldsymbol{\sin\in\to} \\
\end{array}
$$

#### 8.3.4 斜体（意大利体）

用`\mathit{text}`或`{\it text}`或`{\mit text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathit} & \mathit{SAMPLE} & \mathit{sample} & \mathit{0123} & \mathit{\delta\theta\sigma\omega} & \mathit{\Delta\Theta\Sigma\Omega} \\
\hline
\text{it} & {\it SAMPLE} & {\it sample} & {\it 0123} & {\it \delta\theta\sigma\omega} & {\it \Delta\Theta\Sigma\Omega} \\
\hline
\text{mit} & {\mit SAMPLE} & {\mit sample} & {\mit 0123} & {\mit \delta\theta\sigma\omega} & {\mit \Delta\Theta\Sigma\Omega} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathit} & \mathit{SAMPLE} & \mathit{sample} & \mathit{0123} & \mathit{\delta\theta\sigma\omega} & \mathit{\Delta\Theta\Sigma\Omega} \\
\hline
\text{it} & {\it SAMPLE} & {\it sample} & {\it 0123} & {\it \delta\theta\sigma\omega} & {\it \Delta\Theta\Sigma\Omega} \\
\hline
\text{mit} & {\mit SAMPLE} & {\mit sample} & {\mit 0123} & {\mit \delta\theta\sigma\omega} & {\mit \Delta\Theta\Sigma\Omega} \\
\end{array}
$$

可以看到`\mathit`与`\it`等效；对于*拉丁字母、小写希腊字母*，默认字体即为`\mit`。

#### 8.3.5 罗马体

用`\mathrm{text}`或`{\rm text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathrm} & \mathrm{SAMPLE} & \mathrm{sample} & \mathrm{0123} & \mathrm{\delta\theta\sigma\omega} & \mathrm{\Delta\Theta\Sigma\Omega} \\
\hline
\text{rm} & {\rm SAMPLE} & {\rm sample} & {\rm 0123} & {\rm \delta\theta\sigma\omega} & {\rm \Delta\Theta\Sigma\Omega} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathrm} & \mathrm{SAMPLE} & \mathrm{sample} & \mathrm{0123} & \mathrm{\delta\theta\sigma\omega} & \mathrm{\Delta\Theta\Sigma\Omega} \\
\hline
\text{rm} & {\rm SAMPLE} & {\rm sample} & {\rm 0123} & {\rm \delta\theta\sigma\omega} & {\rm \Delta\Theta\Sigma\Omega} \\
\end{array}
$$

可以看到*小写希腊字母*不支持罗马体；对于*阿拉伯数字、大写希腊字母*，默认字体即为`\rm`。

#### 8.3.6 等线体/无衬线体

用`\mathsf{text}`或`{\sf text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathsf} & \mathsf{SAMPLE} & \mathsf{sample} & \mathsf{0123} & \mathsf{\delta\theta\sigma\omega} & \mathsf{\Delta\Theta\Sigma\Omega} \\
\hline
\text{sf} & {\sf SAMPLE} & {\sf sample} & {\sf 0123} & {\sf \delta\theta\sigma\omega} & {\sf \Delta\Theta\Sigma\Omega} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathsf} & \mathsf{SAMPLE} & \mathsf{sample} & \mathsf{0123} & \mathsf{\delta\theta\sigma\omega} & \mathsf{\Delta\Theta\Sigma\Omega} \\
\hline
\text{sf} & {\sf SAMPLE} & {\sf sample} & {\sf 0123} & {\sf \delta\theta\sigma\omega} & {\sf \Delta\Theta\Sigma\Omega} \\
\end{array}
$$

可以看到*小写希腊字母*不支持无衬线体。

#### 8.3.7 手写体

用`\mathscr{text}`或`{\scr text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathscr} & \mathscr{SAMPLE} & \mathscr{sample} & \mathscr{0123} & \mathscr{\delta\theta\sigma\omega} & \mathscr{\Delta\Theta\Sigma\Omega} \\
\hline
\text{scr} & {\scr SAMPLE} & {\scr sample} & {\scr 0123} & {\scr \delta\theta\sigma\omega} & {\scr \Delta\Theta\Sigma\Omega} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathscr} & \mathscr{SAMPLE} & \mathscr{sample} & \mathscr{0123} & \mathscr{\delta\theta\sigma\omega} & \mathscr{\Delta\Theta\Sigma\Omega} \\
\hline
\text{scr} & {\scr SAMPLE} & {\scr sample} & {\scr 0123} & {\scr \delta\theta\sigma\omega} & {\scr \Delta\Theta\Sigma\Omega} \\
\end{array}
$$

可以看到*阿拉伯数字、大写希腊字母*显示结果与斜体等效；*小写希腊字母*不改变。

#### 8.3.8 花体

用`\mathcal{text}`或`{\cal text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathcal} & \mathcal{SAMPLE} & \mathcal{sample} & \mathcal{0123} & \mathcal{\delta\theta\sigma\omega} & \mathcal{\Delta\Theta\Sigma\Omega} \\
\hline
\text{cal} & {\cal SAMPLE} & {\cal sample} & {\cal 0123} & {\cal \delta\theta\sigma\omega} & {\cal \Delta\Theta\Sigma\Omega} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathcal} & \mathcal{SAMPLE} & \mathcal{sample} & \mathcal{0123} & \mathcal{\delta\theta\sigma\omega} & \mathcal{\Delta\Theta\Sigma\Omega} \\
\hline
\text{cal} & {\cal SAMPLE} & {\cal sample} & {\cal 0123} & {\cal \delta\theta\sigma\omega} & {\cal \Delta\Theta\Sigma\Omega} \\
\end{array}
$$

可以看到*阿拉伯数字、大写希腊字母*显示结果与斜体等效；*小写拉丁字母、小写希腊字母*不改变。

#### 8.3.9 打字机体

用`\mathtt{text}`或`{\tt text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathtt} & \mathtt{SAMPLE} & \mathtt{sample} & \mathtt{0123} & \mathtt{\delta\theta\sigma\omega} & \mathtt{\Delta\Theta\Sigma\Omega} \\
\hline
\text{tt} & {\tt SAMPLE} & {\tt sample} & {\tt 0123} & {\tt \delta\theta\sigma\omega} & {\tt \Delta\Theta\Sigma\Omega} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 & \delta\theta\sigma\omega & \Delta\Theta\Sigma\Omega \\
\hline
\text{mathtt} & \mathtt{SAMPLE} & \mathtt{sample} & \mathtt{0123} & \mathtt{\delta\theta\sigma\omega} & \mathtt{\Delta\Theta\Sigma\Omega} \\
\hline
\text{tt} & {\tt SAMPLE} & {\tt sample} & {\tt 0123} & {\tt \delta\theta\sigma\omega} & {\tt \Delta\Theta\Sigma\Omega} \\
\end{array}
$$

可以看到*小写希腊字母*不支持打字机体。

#### 8.3.10 Fraktur体/德国哥特体

用`\mathfrak{text}`或`{\frak text}`表示，注意控制范围。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{mathfrak} & \mathfrak{SAMPLE} & \mathfrak{sample} & \mathfrak{0123} \\
\hline
\text{frak} & {\frak SAMPLE} & {\frak sample} & {\frak 0123} \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{mathfrak} & \mathfrak{SAMPLE} & \mathfrak{sample} & \mathfrak{0123} \\
\hline
\text{frak} & {\frak SAMPLE} & {\frak sample} & {\frak 0123} \\
\end{array}
$$

#### 8.3.11 小型手写体

用`{\scriptstyle text}`命令，同时可以嵌套其他字体。

```latex
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{scriptstyle} & {\scriptstyle SAMPLE} & {\scriptstyle sample} & {\scriptstyle 0123} \\
\hline
\text{scriptstyle+text} & {\scriptstyle \text{SAMPLE} } & {\scriptstyle \text{sample} } & {\scriptstyle \text{0123} } \\
\end{array}
```

$$
\begin{array}{c|ccccc}
\text{defalut} & SAMPLE & sample & 0123 \\
\hline
\text{scriptstyle} & {\scriptstyle SAMPLE} & {\scriptstyle sample} & {\scriptstyle 0123} \\
\hline
\text{scriptstyle+text} & {\scriptstyle \text{SAMPLE} } & {\scriptstyle \text{sample} } & {\scriptstyle \text{0123} } \\
\end{array}
$$

### 8.4 混合字体

正常情况下，拉丁字母会被当做变量斜体显示。若需要非斜体显示，可以用`\text`命令。

```latex
abc \quad \text{abc}
```

$ abc \quad \text{abc} $​

`\text`中仍可以使用` $ 公式 $ `插入公式。

```latex
f(n)=
\begin{cases}
n/2, & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
```

$$
f(n)=
\begin{cases}
n/2, & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
$$

混合输入时，注意空格显示。可以使用`~`或`\`加空格强制显示空格。

```latex
\begin{matrix}
\text{if} n \text{is even} \\ % Bad
\text{if } n \text{ is even} \\ % Good
\text{if}~n~\text{is even} \\ % Good
\text{if}\ n\ \text{is even} \\ % Good
\end{matrix}
```

$$
\begin{matrix}
\text{if} n \text{is even} \\ % Bad
\text{if } n \text{ is even} \\ % Good
\text{if}~n~\text{is even} \\ % Good
\text{if}\ n\ \text{is even} \\ % Good
\end{matrix}
$$

## 9 括号

圆括号`()`、方括号`[]`表示其本身，花括号`{}`需要`\`转义表示。

`\left`和`\right`命令用来生成自动匹配高度括号或括号型字符。

```latex
(\dfrac{1}{2})^2 \quad \left( \dfrac{1}{2} \right) ^2
```

$ (\dfrac{1}{2})^2 \quad \left( \dfrac{1}{2} \right) ^2 $

### 9.1 圆括号、方括号、花括号

```latex
\left( \dfrac ab \right) \quad \left[ \dfrac ab \right] \quad \left\{ \dfrac ab \right\}
```

$ \left( \dfrac ab \right) \quad \left[ \dfrac ab \right] \quad \left\{ \dfrac ab \right\} $

### 9.2 角括号、单竖线（绝对值）、双竖线（范数）

```latex
\left\langle \dfrac ab \right\rangle \quad \left| \dfrac ab \right| \quad \left\| \dfrac ab \right\|
```

$ \left\langle \dfrac ab \right\rangle \quad \left| \dfrac ab \right| \quad \left\| \dfrac ab \right\| $

### 9.3 取整函数

```latex
\left\lfloor \dfrac ab \right\rfloor \quad \left\lceil \dfrac ab \right\rceil
```

$ \left\lfloor \dfrac ab \right\rfloor \quad \left\lceil \dfrac ab \right\rceil $

### 9.4 斜线与箭头

```latex
\left/ \dfrac ab \right\backslash
```

$ \left/ \dfrac ab \right\backslash $

```latex
\left\uparrow \dfrac ab \right\downarrow \quad \left\Uparrow \dfrac ab \right\Downarrow \quad \left\updownarrow \dfrac ab \right\Updownarrow
```

$ \left\uparrow \dfrac ab \right\downarrow \quad \left\Uparrow \dfrac ab \right\Downarrow \quad \left\updownarrow \dfrac ab \right\Updownarrow $

### 9.5 混合括号

```latex
\left[ \dfrac{1}{2}, 1 \right) \quad \left\langle \psi \right|
```

$ \left[ \dfrac{1}{2}, 1 \right) \quad \left\langle \psi \right| $

### 9.6 单边括号

用`\left.`或`\right.`匹配另一边。

```latex
\left\{
\begin{array}{l}
x+y=3, \\
2x+3y=8
\end{array}
\right.
```

$ \left\{
\begin{array}{l}
x+y=3, \\
2x+3y=8
\end{array}
\right. $

```latex
\left. \dfrac ab \right\}
```

$ \left. \dfrac ab \right\} $

### 9.7 控制括号大小

使用`\big`、`\Big`、`\bigg`、`\Bigg`控制括号大小。

```latex
\left\{ \left[ \left( \left| \left\| a \right\| +1 \right| -2 \right) +3 \right] -4 \right\}*5
```

$ \left\{ \left[ \left( \left| \left\| a \right\| +1 \right| -2 \right) +3 \right] -4 \right\}*5 $​

```latex
\Bigg\{ \bigg[ \Big( \big| \left\| a \right\| +1 \big| -2 \Big) +3 \bigg] -4 \Bigg\}*5
```

$ \Bigg\{ \bigg[ \Big( \big| \left\| a \right\| +1 \big| -2 \Big) +3 \bigg] -4 \Bigg\}*5 $

## 10 空格

LaTeX会忽略公式中的空格，空格控制从宽到窄依次为：

**双quad空格**：2个字符宽

```latex
\alpha \qquad \beta
```

$ \alpha \qquad \beta $

**quad空格**：1个字符宽

```latex
\alpha \quad \beta
```

$ \alpha \quad \beta $​

**空格**：1/3个字符宽

```latex
\alpha \ \beta ~ \gamma
```

$ \alpha \ \beta ~ \gamma $

**中空格**：2/7个字符宽

```latex
\alpha \; \beta
```

$ \alpha \; \beta $​

**小空格**：1/6个字符宽

```latex
\alpha \, \beta
```

$ \alpha \, \beta $​

**无空格**：0个字符宽

```latex
\alpha \beta
```

$ \alpha \beta $​

**紧贴**：-1/6个字符宽

```latex
\alpha \! \beta
```

$ \alpha \! \beta $​

## 11 颜色

使用`{\color{color}{text} }`更改文字颜色，注意控制范围。

```latex
\color{red}{text} \quad \color{yellow}{text} \quad \color{blue}{text} \quad \color{green}{text} \quad \color{purple}{text}
```

$ \color{red}{text} \quad \color{yellow}{text} \quad \color{blue}{text} \quad \color{green}{text} \quad \color{purple}{text} $​

color名小写时表示简单色调，首字母大写时表示较为复杂的色调。

```latex
\color{Red}{text} \quad \color{Orange}{text} \quad \color{RoyalBlue}{text} \quad \color{Violet}{text} \quad \color{LimeGreen}{text}
```

$ \color{Red}{text} \quad \color{Orange}{text} \quad \color{RoyalBlue}{text} \quad \color{Violet}{text} \quad \color{LimeGreen}{text} $

使用`{\color{ #rgb}{text} }`选择更多颜色，`rgb`的范围是`0-9`、`A-F`。

```latex
\color{ #0FF}{text} \quad \color{ #00F}{text} \quad \color{ #F0F}{text} \quad \color{ #0F0}{text} \quad \color{ #6CF}{text}
```

$ \color{ #0FF}{text} \quad \color{ #00F}{text} \quad \color{ #F0F}{text} \quad \color{ #0F0}{text} \quad \color{ #6CF}{text} $

## 12 小技巧

1. 如何打出如下格式？

   $ \mathop{x} \limits_a^b $

   由于`\limits`只能用在运算符（如`\sum`）后，所以可以用`\mathop`命令使字母变成运算符。

   ```latex
   \mathop{x} \limits_a^b
   ```

   也可以选择`\overset`和`\underset`实现。

   ```latex
   \underset{a}{\overset{b}{x} }
   ```

2. `|`和`\vert`、`\mid`的区别：

   ```latex
   | \quad \vert \quad \mid
   ```

   $ | \quad \vert \quad \mid $​

   分以下情况讨论：

   - **绝对值**

     `|`与`\vert`均可，还可以使用`\lvert`、`\rvert`。注意到下例中，-2左侧的`\vert`被识别为*普通元素*，而非*关系元素*，因此在非匹配情况下，不建议使用`\vert`。

     ```latex
     |-1| \quad \vert -2 \vert \quad \lvert -3 \rvert
     ```

     $ |-1| \quad \vert -2 \vert \quad \lvert -3 \rvert $​

     当遇到分数时，使用`\left`、`\right`匹配高度。注意`\lvert`、`\rvert`并不会匹配高度！

     ```latex
     \lvert \dfrac ab \rvert \quad \left| \dfrac cd \right| \quad \left\vert \dfrac ef \right\vert
     ```

     $ \lvert \dfrac ab \rvert \quad \left| \dfrac cd \right| \quad \left\vert \dfrac ef \right\vert $​

   - **整除**

     用关系符号`\mid`表示。如：

     ```latex
     a \mid b
     ```

     $ a \mid b $

     表示$a$能整除$b$​，即$b$能被$a$整除，如$ 3 \mid 9 $表示9能被3整除。

     注意`\mid`不可伸长，伸长需要借助`|`，使用单边匹配`|`的方法，或`\middle`命令（`\middle`需要左右为`\left.`及`\right.`）。

     ```latex
     \dfrac ab \mid c \quad \left. \dfrac ab \right| c \quad \left. \dfrac ab \middle| c \right.
     ```

     $ \dfrac ab \mid c \quad \left. \dfrac ab \right| c \quad \left. \dfrac ab \middle| c \right. $​

   - **集合**

     同上，应用`\mid`，伸长时用`\middle|`替代。

     ```latex
     \begin{matrix}
     \left\{ x \mid x \in \mathbb{R} \text{ and } x \ne 1 \right\} \\
     \left\{ \dfrac ab \mid a,b \in \mathbb{N} \text{ and } b > 5 \right\} \\
     \left\{ \left. \dfrac ab \middle| a,b \in \mathbb{N} \text{ and } b > 5 \right. \right\}
     \end{matrix}
     ```

     $ \begin{matrix}
     \left\{ x \mid x \in \mathbb{R} \text{ and } x \ne 1 \right\} \\
     \left\{ \dfrac ab \mid a,b \in \mathbb{N} \text{ and } b > 5 \right\} \\
     \left\{ \left. \dfrac ab \middle| a,b \in \mathbb{N} \text{ and } b > 5 \right. \right\}
     \end{matrix} $​

   - **函数**

     因为通常高度不固定，因此建议用`|`表示并匹配高度。

     ```latex
     f'(x_0) = \left. f'(x) \right| _{x=x_0} = \left. \dfrac{\mathrm{d}f}{\mathrm{d}x} \right| _{x=x_0}
     ```

     $ f'(x_0) = \left. f'(x) \right| _{x=x_0} = \left. \dfrac{\mathrm{d}f}{\mathrm{d}x} \right| _{x=x_0} $

   - **双竖线**

     把`|`、`\vert`、`\lvert`、`\rvert`、`\mid`分别替换为`\|`、`\Vert`、`\lVert`、`\rVert`、`\parallel`即可，基本属性与上述对应单竖线类似。

     ```latex
     \| x \| \quad \left\Vert \dfrac ab \right\Vert \quad \lVert c \rVert \quad l \parallel m
     ```

     $ \| x \| \quad \left\Vert \dfrac ab \right\Vert \quad \lVert c \rVert \quad l \parallel m $

3. 如何输入斜分式？

   $ {}^1/_2 $​

   用上下标即可。

   ```latex
   {}^1/_2
   ```

4. 如何输入化学方程式？

   $ \mathrm{CH_3CH_2OH \xrightarrow[170^\circ C]{浓H_2SO_4} CH_2=CH_2 \uparrow + H_2O } $

   使用`\xrightarrow`和已有的符号即可。也可使用宏包，在此不表。

   ```latex
   \mathrm{CH_3CH_2OH \xrightarrow[170^\circ C]{浓H_2SO_4} CH_2=CH_2 \uparrow + H_2O }
   ```

   给箭头加文字有如下方式：

   ```latex
   \begin{matrix}
   \xleftarrow[x+y+z]{x+y+z+1} \\ % 自动调整长度
   \xrightarrow[x+y+z]{x+y+z+1} \\ % 自动调整长度
   \overset{x+y}{\rightarrow} \\ % 长度固定
   \underrightarrow{x+y+z} \\ % 自动调整长度
   \underset{x+y}{\leftarrow} \\ % 长度固定
   \overleftarrow{x+y+z} \\ % 自动调整长度
   \end{matrix}
   ```

   $ \begin{matrix}
   \xleftarrow[x+y+z]{x+y+z+1} \\ % 自动调整长度
   \xrightarrow[x+y+z]{x+y+z+1} \\ % 自动调整长度
   \overset{x+y}{\rightarrow} \\ % 长度固定
   \underrightarrow{x+y+z} \\ % 自动调整长度
   \underset{x+y}{\leftarrow} \\ % 长度固定
   \overleftarrow{x+y+z} \\ % 自动调整长度
   \end{matrix} $

5. 如何给公式加方框？

   用`\boxed`命令或用1*1表格的边框。

   ```latex
   \boxed {E=mc^2}
   ```

   $ \boxed {E=mc^2} $

   ```latex
   \begin{array}{|c|}
   \hline
   E=mc^2 \\
   \hline
   \end{array}
   ```

   $ \begin{array}{|c|}
   \hline
   E=mc^2 \\
   \hline
   \end{array} $

6. 如何输入分数的约分形式/如何输入删除线？

   $ \require{cancel} \dfrac{\cancel{15}3}{\cancel{25}5} = \dfrac{3}{5} $

   可以使用`\cancel`、`\bcancel`、`\xcancel`和`\cancelto`命令（需要`\require`导包）。

   ```latex
   \require{cancel}
   \begin{array}{rl}
   \verb|y+\cancel{x}| & y+\cancel{x} \\
   \verb|\cancel{y+x}| & \cancel{y+x} \\
   \verb|y+\bcancel{x}| & y+\bcancel{x} \\
   \verb|y+\xcancel{x}| & y+\xcancel{x} \\
   \verb|y+\cancelto{0}{x}| & y+\cancelto{0}{x} \\
   \end{array}
   ```

   $$
   \require{cancel}
   \begin{array}{rl}
   \verb|y+\cancel{x}| & y+\cancel{x} \\
   \verb|\cancel{y+x}| & \cancel{y+x} \\
   \verb|y+\bcancel{x}| & y+\bcancel{x} \\
   \verb|y+\xcancel{x}| & y+\xcancel{x} \\
   \verb|y+\cancelto{0}{x}| & y+\cancelto{0}{x} \\
   \end{array}
   $$
   
   ```latex
   \require{cancel}
   \begin{array}{c}
   \verb+\dfrac{\cancel{15}3}{\cancel{25}5} = \dfrac{3}{5}+ \\
   \dfrac{\cancel{15}3}{\cancel{25}5} = \dfrac{3}{5} \\
   \end{array}
   ```
   
   $$
   \require{cancel}
   \begin{array}{c}
   \verb+\dfrac{\cancel{15}3}{\cancel{25}5} = \dfrac{3}{5}+ \\
   \dfrac{\cancel{15}3}{\cancel{25}5} = \dfrac{3}{5} \\
   \end{array}
   $$
   
   上例还使用了`\verb`命令来显示原文照排效果。

## 13 示例

以下公式来源于：Tsung-Chyan Lai,Yuri N. Sotskov,Alexandre Dolgui. The stability radius of an optimal line balance with maximum efficiency for a simple assembly line[J]. European Journal of Operational Research,2018,274(2):

1. 导数、括号匹配、上下标、特殊符号、大型运算符
   
   ```latex
   t' \left( V_k^{b_r} \right) := \sum_{i \in V_k^{b_r} } t_i' = \sum_{i \in V_k^{b_r} \setminus \left\{ j \right\} } t_i'
   ```
   
   $$
   t' \left( V_k^{b_r} \right) := \sum_{i \in V_k^{b_r} } t_i' = \sum_{i \in V_k^{b_r} \setminus \left\{ j \right\} } t_i'
   $$
   
2. 希腊字母、括号匹配、特殊符号、字母标记
   
   ```latex
   \delta_{b_1u}^{=b_rk} = \frac{t \left( V_k^{b_r} \right) - t \left( V_u^{b_1} \right)}{\left| \tilde{V}_k^{b_r} \oplus \tilde{V}_u^{b_1} \right|}
   ```
   
   $$
   \delta_{b_1u}^{=b_rk} = \frac{t \left( V_k^{b_r} \right) - t \left( V_u^{b_1} \right)}{\left| \widetilde{V}_k^{b_r} \oplus \widetilde{V}_u^{b_1} \right|}
   $$
   
3. 希腊字母、特殊符号、条件表达式、函数、正文字体
   
   ```latex
   \gamma \left( b_r \right) :=
   \begin{cases}
   \frac{1}{\tilde{n} } \cdot \left[ c \left( b_r, t \right) -  \max \left\{ t \left( V_k^{b_r} \right) : {\widetilde{V}_k^{b_r} } \notin W \left( b_r, t \right) \right\} \right], & \text{if $k \in \left\{ 1, 2, \ldots, m \left( b_r \right) \right\}$ with $t \left( V_k^{b_r} \right) < c \left( b_r, t \right)$;} \\
   \min \left\{ t_i : i \in \widetilde{V} \right\}, & \text{if $t \left( V_k^{b_r} \right) = c \left( b_r, t \right)$ for each $k \in \left\{ 1, 2, \ldots, m \left( b_r \right) \right\}$.}
   \end{cases}
   ```
   
   $$
   \gamma \left( b_r \right) :=
   \begin{cases}
   \frac{1}{\tilde{n} } \cdot \left[ c \left( b_r, t \right) -  \max \left\{ t \left( V_k^{b_r} \right) : {\widetilde{V}_k^{b_r} } \notin W \left( b_r, t \right) \right\} \right], & \text{if $k \in \left\{ 1, 2, \ldots, m \left( b_r \right) \right\}$ with $t \left( V_k^{b_r} \right) < c \left( b_r, t \right)$;} \\
   \min \left\{ t_i : i \in \widetilde{V} \right\}, & \text{if $t \left( V_k^{b_r} \right) = c \left( b_r, t \right)$ for each $k \in \left\{ 1, 2, \ldots, m \left( b_r \right) \right\}$.}
   \end{cases}
   $$

## 14 小工具

- 手画符号搜索LaTeX代码：http://detexify.kirelabs.org/classify.html
- LaTeX在线编辑器：<http://www.codecogs.com/latex/eqneditor.php>

