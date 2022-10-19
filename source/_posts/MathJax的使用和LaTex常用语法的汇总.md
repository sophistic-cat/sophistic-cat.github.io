---
title: MathJax的使用 和 LaTex常用语法的汇总
date: 2020-06-25 16:36:02
tags: [MathJax, LaTex]
---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
    tex2jax: {inlineMath: [['$','$'], ['\(','\)']]}
    });
</script>

# 引言
__·__ 之前写博客的时候总有数学公式要写，这时就要用到MathJax且要熟知Latex的相关语法了，这次就简单整理下相关知识。
# MathJax
## 1. 什么是MathJax？
> __MathJax__是一个跨浏览器的JavaScript库，它使用MathML、LATEX和ASCIIMathML标记在Web浏览器中显示数学符号。<p align="right">__----------------------wikipedia__</p>  
<!-- more -->

## 2. 在MarkDown中引入MathJax插件
__·__ 一般来说，在MarkDown中引入MathJax插件后，才可正常显示LaTex公式。
```js
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
```
__PS:__　但是上面有一个问题，这样引用的话是可以使用 `$$ LaTex公式 $$` 的方法来写单独段落的公式，但是，如果你要用 `$ LaTeX公式 $` 来写行内公式，就会发现并不支持这种用法。
__解决方案:__　引入下面的代码。
```js
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({
    tex2jax: {inlineMath: [['$','$'], ['\(','\)']]}
    });
</script>
```
## 3. 在MarkDown中插入公式
__· 行内插入公式：__  ` $LaTex公式$ `

　　__EXP：__`$ a_1+5^2+\sqrt{x^3} $`　效果：$a_1+5^2+\sqrt{x^3}$
　　
__· 单独段落的公式：__  ` $$LaTex公式$$ `

　　__EXP：__`$$ a_1+5^2+\sqrt{x^3} $$`　效果：$$a_1+5^2+\sqrt{x^3}$$

__Ps：__ 单独段落显示时，公式会居中且放大显示。

# LaTex
## 1. 什么是LaTex？
> __LaTex__，是一种基于TEX的排版系统，由美国计算机科学家莱斯利·兰伯特在20世纪80年代初期开发，利用这种格式系统的处理，即使用户没有排版和程序设计的知识也可以充分发挥由TEX所提供的强大功能，不必一一亲自去设计或校对，能在几天，甚至几小时内生成很多具有书籍质量的印刷品。对于生成复杂表格和数学公式，这一点表现得尤为突出。因此它非常适用于生成高印刷质量的科技和数学、物理文档。这个系统同样适用于生成从简单的信件到完整书籍的所有其他种类的文档。<p align="right">__----------------------wikipedia__</p>  

## 2. LaTex部分语法总结
__[注1]__　这是我写博客是遇到过的问题，有时候LaTex中涉及`\`的命令可能会失效，这一点有点诡异，因为这个问题时有时无，到现在还没找到问题原因所在（可能是有时候需要对`\`进行转义？不太清清楚），emmmm，反正当语法不能实现时，可以试下对`\`转义，一般就能解决.如：本来`\\`表示换行，但却没有实现，而当使用`\\\`后才能表示换行，这点要特别注意。
__[注2]__　下面的一些符号也可以直接通过键盘或输入法输入。
### 数学符号
#### 运算符号
|符号|使用方法|效果|
|:---:|:---:|:---:|
|+|`+`|$+$|
|-|`-`|$-$|
|×|`\times`|$\times$|
|÷|`\div`|$\div$|
|±|`\pm`|$\pm$|
|∓|`\mp`|$\mp$|

#### 关系比较符号
|符号|使用方法|效果|
|:---:|:---:|:---:|
|>|`\lt`|$\lt$|
|<|`\gt`|$\gt$|
|≥|`\ge`|$\ge$|
|≤|`\le`|$\le$|
|≠|`\neq`|$\neq$|

#### 箭头符号
|符号|使用方法|效果|
|:---:|:---:|:---:|
|→|`\to`|$\to$|
|→|`\rightarrow`|$\rightarrow$|
|←|`\rightarrow`|$\leftarrow$|
|⇒|`\Rightarrow`|$\Rightarrow$|
|⇐|`\Leftarrow`|$\Leftarrow$|
|↦|`\mapsto`|$\mapsto$|
|⇑|`\Uparrow`|$\Uparrow$|
|↑|`\uparrow`|$\uparrow$|
|⇓|`\Downarrow`|$\Downarrow$|
|↓|`\downarrow`|$\downarrow$|

Ps：__长剪头__就加long即可，如：`$\longrightarrow \Longrightarrow \longmapsto$`：　效果：$\longrightarrow \Longrightarrow \longmapsto$


#### 分组
|使用方法|作用|例子|
|:---:|:---:|:---:|
|`{}`|通过`{}`将操作数和符号分割开，消除二义性|`$a^10$`:　$a^10$<br>`$a^{10}$`:　$a^{10}$|

#### 上下标
|名称|使用方法|例子|
|:---:|:---:|:---:|
|上标|`^`|`$2^2=4$`:　$2^2=4$|
|下标|`_`|`$a_1=a_2$`:　$a_1=a_2$|

#### 段外效果

|命令|应用范围|例子|
|:---:|:---:|:---:|
|`\limits`|__数学运算符的段外效果__|`$\sum\nolimits_{n=1}^N a_n$`　$\prod\limits_{n=1}^N a_n$|
|`\mathop`+`\limits`|先用\mathop转换为数学符号再用\limits就可以实现__普通符号的段外效果__|`\mathop{C}\limits_{a}^{b}`　$\mathop{C}\limits_{a}^{b}$|

__ps:__　顺便提一下__`\nolimits`__命令，该命令实现的是上下标的效果，如：`$\sum\nolimits_{n=1}^{N} a_n$`　$\sum\nolimits_{n=1}^{N} a_n$

#### 相对位置
__·__ __`\stackrel{正上方}{正中方}`__
__·__ 例子：`` $X\stackrel{+1}{\Longrightarrow}Y$

#### 空间
· 直接在两个元素之间加入空格是没用的，MathJax有自己的策略来决定公式的空间距离。

|名称|使用方法|例子|
|:---:|:---:|:---:|
|一点的距离|`\,`|$a_1\\,=1$|
|多一点的距离|`\;`|$a_1\\;=1$|
|大的距离|`\quad`|$a_1\quad=1$|
|更大的距离|`\qquad`|$a_1\qquad=1$|

四种方式的对比：（依次为：`\,` `\;` `\quad` `\qquad`）
$a_1\\,=1$
$a_1\\;=1$
$a_1\quad=1$
$a_1\qquad=1$

#### 分式
__· 写法一：__使用`\frac ab` 如：`\frac {1+a^2}{1-b}`，效果为：$\frac {1+a^2}{1-b}$
__· 写法二：__使用`a \over b` 如：`{1+a^2} \over {1-b}`，效果为：${1+a^2} \over {1-b}$
　__ps：__　推荐第二种写法

#### 根式
|名称|使用方法|例子|
|:---:|:---:|:---:|
|平方根|`\sqrt x`|<br>代码：__`$\sqrt{x^5}$`__<hr>效果：$\sqrt{x^5}$<br>|
|其余根式|`\sqrt[n] x`|<br>代码：__`$\sqrt[5]{x^7 \over 32}`__<hr>效果：$\sqrt[5]{x^7 \over 32}$|

#### 对齐
__·__ __`&`__
__·__ 例子：`$$\begin{\end.$$`


$\left\\{ \right.$

#### 求和、求积、极限与积分
|符号|名称|使用方法|例子|
|:---:|:---:|:---:|:---:|
|∑|求和|`\sum`|`$\sum$`：　$\sum_{k=-\infty}^{\infty}X{K\Omega}$|
|∏|求积|`\prod`|`$\prod_{i=1}^{n}{i+1}$`：　$\prod_{i=1}^{n}{2(i+1)}$|
|lim|极限|`\lim`|`$\lim\limits_{n\to\infty}}$`：　$\lim\limits_{n \to {0^{+}}} {\sin x \over x}$|
|∫|积分|`\int`|`$\int_{0}^{1}\sin{(x+1)}dx$`：　$\int_{0}^{1}\sin{(x+1)}dx$|
|∫∫|二重积分|`\iint`|`$\iint\limits_D f(x, y)d\sigma$`：　$\iint\limits_D f(x, y)d\sigma$|
|∫∫∫|三重积分|`\iiint`|`$\iiint\limits_\Omega f(x, y, z)dv$`：　$\iiint\limits_\Omega f(x, y, z)dv$|

#### 特殊函数
__·__ 命令：__`\函数名`__

|名称|使用方法|例子|
|:---:|:---:|:---:|
|正弦|`\sin`|`$\sin \theta$`：　$\sin \theta$|
|余弦|`\cos`|`$\cos \theta$`：　$\cos \theta$|
|正切|`\tan`|`$\tan \theta$`：　$\tan \theta$|
|反正弦|`\arcsin`|`$\arcsin {1 \over x}$`：　$\arcsin {1 \over x}$|
|反余弦|`\arccos`|`$\arccos x^2$`：　$\arccos x^2$|
|反正切|`\arctan`|`$\arctan {x_1 \over y_1}$`：　$\arctan {x_1 \over y_1}$|
|对数函数|`\log`|`$\log_a x$`：　$\log_a x$|
|ln|`\ln`|`$\ln e$`：　$\ln e$|
|Max函数|`\max`|`$\max (A,B,C)$`：　$\max (A,B,C)$|
|Min函数|`\min`|`$\min (A,B,C)$`：　$\min(A,B,C)$|

#### 多行列式
##### 方程组
__·__ 方法一：　`\begin{cases} -方程组- \end{cases}`
__·__ 方法二：　`\left\{ -方程组- \right.`

__Ps:__　这里方法二就出现了我前面说的问题，这里直接`\left\{`会出问题，需要改成`\left\\{`

|命令|效果|
|:---:|:---:|
|`$\begin{cases} a_1x+b_1y+c_1z=d_1 \\\ a_2x+b_2y+c_2z=d_2 \\\ a_3x+b_3y+c_3z=d_3 \end{cases}$`|$\begin{cases} a_1x+b_1y+c_1 z=d_1 \\\ a_2x+b_2y+c_2z=d_2 \\\ a_3x+b_3y+c_3z=d_3 \end{cases}$|
|`$\left\\{ a_1x+b_1y+c_1z=d_1 \\\ a_2x+b_2y+c_2z=d_2 \\\ a_3x+b_3y+c_3z=d_3 \right.$`|$\left\\{ \begin{align} a_1x+b_1y+c_1z=d_1 \\\ a_2x+b_2y+c_2z=d_2 \\\ a_3x+b_3y+c_3z=d_3 \end{align} \right.$|

$$
\begin{cases}
a_1+b_1=1 \\\ 
a_2+b_2=2 \\\ 
\end{cases}
$$

##### 多行等式

### 希腊字母
__·__ 想来想去还是要补一下希腊字母的，毕竟α、β、γ啥的用的还是挺多的

|名称|读音|使用方法|效果|名称|读音|使用方法|效果|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|α|阿尔法|`\alpha`|$\alpha$|β|贝塔|`\beta`|$\beta$|
|γ|伽马|`\gamma`|$\gamma$|δ|德尔塔|`\delta`|$\delta$|
|ε|伊普西龙|`\epsilon`|$\epsilon$|ζ|捷塔|`\zeta`|$\zeta$|
|η|依塔|`\eta`|$\eta$|θ|西塔|`\theta`|$\theta$|
|ι|艾欧塔|`\iota`|$\iota$|κ|喀帕|`\kappa`|$\kappa$|
|λ|拉姆达|`\lambda`|$\lambda$|μ|缪|`\mu`|$\mu$|
|ν|拗|`\nu`|$\nu$|ξ|柯西|`\xi`|$\xi$|
|ο|欧麦克轮|`\omicron`|$\omicron$|π|派|`\pi`|$\pi$|
|ρ|肉|`\rho`|$\rho$|σ|西格玛|`\sigma`|$\sigma$|
|τ|套|`\tau`|$\tau$|υ|宇普西龙|`\upsilon`|$\upsilon$|
|φ|fai|`\phi`|$\phi$|χ|器|`\chi`|$\chi$|
|ψ|普赛|`\psi`|$\psi$|ω|欧米伽|`\omega`|$\omega$|

# 总结
__·__ 结合官网、他人博客以及亲身实践等总结了这篇文章，并不是那么全面，但目前对我来说也够用，以后遇到其他的再补充吧。

# 参考文献
__·__ [维基百科](https://zh.wikipedia.org/wiki/Wikipedia:%E9%A6%96%E9%A1%B5)
__·__ [LaTex官网帮助文件](https://www.latex-project.org/help/documentation/amsldoc.pdf)
__·__ [MathJax基本的使用方式 By sam-X](https://blog.csdn.net/u010945683/article/details/46757757)
