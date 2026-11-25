---
# 文章标题（列表与页面顶部显示；文件名只决定 URL）
title: 'Markdown × LaTeX 渲染自检'

# 文章创建日期
date: 2026-11-19T21:40:00+08:00

# 草稿：false 才会发布
draft: false

# 公式渲染开关：true 时本页才加载 KaTeX
math: true

# 文章标签
tags: ["Markdown", "LaTeX", "KaTeX", "Hugo", "渲染测试"]

# 文章作者
author: ["Rainie Forever"]

# 摘要，用于列表页与 SEO
description: '一篇逐项验收 Markdown 排版与 KaTeX 公式渲染的测试文章：标题、列表、表格、代码、脚注、折叠块、行内与独立公式，以及本站的已知限制'

# 权重，数值越大越靠前
weight: 1

# 展示控制
showToc: false
TocOpen: true
hidemeta: false
comments: true
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true

# 编辑链接
editPost:
    URL: "https://github.com/yyc001/yyc001.github.io/blob/source/content/"
    Text: "View Source"
    appendFilePath: true
---

> 这篇文章是一次“渲染验收”：每一节只放一类语法，发布后逐段对照，就能确认 Hugo、PaperMod 和 KaTeX 都还在正常工作。
>
> 公式依赖 front matter 里的 `math: true`，本页已开启。

## 一、标题层级

一级标题由 front matter 的 `title` 生成，正文从 `##` 开始；正文里再写 `#` 会出现第二个一级标题，文档大纲就不对了。

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

标题右侧的 `#` 是主题注入的锚点，点击可以复制小节链接。

## 二、文本样式

普通文字里混排 **粗体**、*斜体*、***粗斜体***、~~删除线~~ 和 `行内代码`。

粗体里也能放公式：**能量守恒 $E = mc^2$ 在最粗的字重里同样能渲染**。

反斜杠转义：\*这里的星号不构成斜体\*，\_下划线\_ 也保持原样。

行内代码里的符号不会被解析：`**不是粗体**`、`~~不是删除线~~`、`$不是公式$`。

## 三、段落与换行

普通换行（源码里回车一次）不会产生新段落，浏览器会把上下两行接在同一段里。
这仍然是上一段的一部分。

这一行结尾有两个空格，  
所以这里会强制换行。

这一行结尾是一个反斜杠，\
所以这里也会换行，而且源码里看得见。

紧跟在段落下一行的 `---` 会把上一行变成二级标题（setext 语法），所以分隔线要空一行再写：

---

上面这条水平线就是三个连字符单独成行。

## 四、列表

无序列表，嵌套项缩进两个空格：

- 第一项
- 第二项
  - 嵌套项
    - 再嵌一层
- 第三项，结尾带公式 $\sum_{i=1}^{n} i$

有序列表：

1. 第一步
2. 第二步
   1. 子步骤
   2. 另一个子步骤
3. 第三步

从指定编号开始：

3. 从 3 开始
4. 继续
5. 结束

任务列表（复选框由 Goldmark 的 task list 扩展生成）：

- [x] 已完成的事项
- [ ] 未完成的事项
- [ ] 带公式的任务：验证 $\int_0^1 x^2\,\mathrm{d}x = \frac{1}{3}$

定义列表：

术语
: 定义内容可以有多行，第二行只要保持缩进就仍属于同一个定义。

Markdown
: 一种轻量级标记语言。

LaTeX
: 排版系统；本站只用它的数学模式，交给 KaTeX 在浏览器里渲染。

## 五、引用

> 单层引用。
>
> 引用里可以放列表：
>
> - 列表项
> - 另一个列表项
>
> > 嵌套引用。
> >
> > 嵌套里也能写公式：$e^{i\pi} + 1 = 0$。

> **提示**：引用块没有额外样式，只保留左侧竖线；多段引用要用空 `>` 行隔开。

## 六、链接

行内链接：[Hugo 官方文档](https://gohugo.io/)；带 title 的链接：[KaTeX](https://katex.org/ "KaTeX 官方网站")。

自动链接：<https://github.com/adityatelange/hugo-PaperMod>

引用式链接：[PaperMod][pm]，定义写在下面，渲染后不可见。

[pm]: https://github.com/adityatelange/hugo-PaperMod "PaperMod 主题仓库"

## 七、图片

Markdown 图片语法，路径指向 `static/images/`，构建后是 `/images/...`：

![渲染测试图片](/images/rendering-test.svg "由 static/images/rendering-test.svg 提供")

Hugo 内置 `figure` 短代码能带标题和说明：

{{< figure src="/images/rendering-test.svg" title="figure 短代码" caption="caption 参数显示在图片下方，title 显示在图片上方。" >}}

## 八、表格

| 左对齐 | 居中 | 右对齐 |
| :--- | :---: | ---: |
| 文本 | 文本 | 文本 |
| `inline code` | $\alpha + \beta$ | 123 |
| **粗体** | *斜体* | ~~删除~~ |

表格里可以混排行内代码与公式；但公式内部不要出现 `|`（例如绝对值），否则会被当成列分隔符。

## 九、代码

行内代码用反引号包住：`git status`、`$HOME`、`\frac{a}{b}`。

内容本身包含反引号时，就多包一层：`` `code` ``。

带语言标注的代码块由 Chroma 高亮：

```python
def fib(n: int) -> int:
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

```bash
# 代码块里的 $ 不会被当成公式
echo $HOME && awk '{print $1}' file.txt
```

```javascript
const delimiters = [
  { left: "$$", right: "$$", display: true },
  { left: "$", right: "$", display: false },
];
```

```latex
% 代码块里写 LaTeX 源码，KaTeX 会跳过 pre / code
\frac{\partial f}{\partial x}
```

```markdown
行内公式写成 $E = mc^2$，独立公式用两个美元符号单独成段。
```

没有语言标注时按纯文本处理：

```
plain text
  indented line
$not math$
```

## 十、脚注

脚注用 `[^1]` 或具名标签声明[^katex]，正文末尾会自动生成脚注区[^passthrough]，点击编号可以来回跳转。

[^katex]: KaTeX 由 `layouts/_partials/extend_head.html` 按需注入，只有 `math: true` 的页面才加载。

[^passthrough]: Goldmark 的 passthrough 扩展让 `$...$` 原样通过，不被 Markdown 解析。

## 十一、折叠块

Hugo 内置 `details` 短代码，标题用 `summary` 参数指定（写成位置参数不生效，会显示默认的 “Details”）：

{{< details summary="点开：折叠块里的公式与代码" >}}
折叠区域同样会经过 Markdown 与 KaTeX：

- 行内公式：$\lim_{n \to \infty} \left(1 + \frac{1}{n}\right)^n = e$
- 行内代码：`renderMathInElement`

$$
\prod_{k=1}^{n} k = n!
$$

折叠块的内容在页面加载时就已经在 DOM 里，KaTeX 会一并渲染，只是默认不可见。
{{< /details >}}

需要默认展开时，加上 `open=true`：

{{< details summary="默认展开的折叠块" open=true >}}
这段内容默认就是展开的。
{{< /details >}}

## 十二、行内公式

行内公式用一对 `$` 包住，和正文共享基线。

质能方程 $E = mc^2$ 写在句子里。

微积分基本定理：$\int_a^b f'(x)\,\mathrm{d}x = f(b) - f(a)$。

高斯积分：$\int_{-\infty}^{\infty} e^{-x^2}\,\mathrm{d}x = \sqrt{\pi}$。

向量范数：$\lVert \mathbf{x} \rVert_2 = \sqrt{\sum_{i=1}^{n} x_i^2}$。

组合数：$\binom{n}{k} = \frac{n!}{k!\,(n-k)!}$。

取整：$\lfloor 3.7 \rfloor = 3$，$\lceil 3.2 \rceil = 4$。

概率：$P(A \mid B) = \dfrac{P(B \mid A)\,P(A)}{P(B)}$。

公式里写文字要放进 `\text{}`：$\text{if } x > 0 \text{ then } \sqrt{x} \in \mathbb{R}$。

`\dfrac`、带上下限的 `\sum` 会把行高撑开，这是正常现象；行内更适合 `\frac`，大公式留给独立公式。

## 十三、独立公式

独立公式用两个 `$` 包住并单独成段，`$$` 与公式之间各留一个换行。

积分：

$$
\int_{0}^{1} x^{2}\,\mathrm{d}x = \frac{1}{3}
$$

求和与极限可以并列写在同一行：

$$
\sum_{k=1}^{\infty} \frac{1}{k^2} = \frac{\pi^2}{6}, \qquad \lim_{x \to 0} \frac{\sin x}{x} = 1
$$

多行推导用 `aligned`，`&` 对齐等号，`\\` 换行：

$$
\begin{aligned}
(a+b)^2 &= a^2 + 2ab + b^2 \\
(a-b)^2 &= a^2 - 2ab + b^2 \\
(a+b)(a-b) &= a^2 - b^2
\end{aligned}
$$

矩阵与行列式：

$$
\mathbf{A} = \begin{pmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{pmatrix},
\qquad
\det(\mathbf{A}) = \begin{vmatrix} a_{11} & a_{12} \\ a_{21} & a_{22} \end{vmatrix}
$$

分段函数用 `cases`：

$$
f(x) =
\begin{cases}
1, & x > 0, \\
0, & x = 0, \\
-1, & x < 0.
\end{cases}
$$

带竖线的增广矩阵（`array` 的列格式里直接写 `|`）：

$$
\left(
\begin{array}{cc|c}
1 & 2 & 3 \\
4 & 5 & 6
\end{array}
\right)
$$

公式编号：KaTeX 没有 `\label` / `\ref` 的自动编号，但可以用 `\tag` 手动标注：

$$
\nabla \cdot \mathbf{E} = \frac{\rho}{\varepsilon_0}
\tag{1}
$$

上下括号：

$$
\underbrace{1 + 2 + \cdots + n}_{n \text{ terms}} = \frac{n(n+1)}{2}
$$

等号上方加标注用 `\overset`：

$$
f'(x) \overset{\text{def}}{=} \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}
$$

给结论加框：

$$
\boxed{\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}}
$$

Maxwell 方程组（顺带测试 `\nabla`、`\partial` 与粗体向量）：

$$
\begin{aligned}
\nabla \cdot \mathbf{E} &= \frac{\rho}{\varepsilon_0}, &
\nabla \cdot \mathbf{B} &= 0, \\
\nabla \times \mathbf{E} &= -\frac{\partial \mathbf{B}}{\partial t}, &
\nabla \times \mathbf{B} &= \mu_0 \mathbf{J} + \mu_0 \varepsilon_0 \frac{\partial \mathbf{E}}{\partial t}.
\end{aligned}
$$

## 十四、符号速查

**希腊字母**

| 小写 | 源码 | 大写 | 源码 |
| --- | --- | --- | --- |
| $\alpha$ | `\alpha` | $\Gamma$ | `\Gamma` |
| $\beta$ | `\beta` | $\Delta$ | `\Delta` |
| $\gamma$ | `\gamma` | $\Theta$ | `\Theta` |
| $\lambda$ | `\lambda` | $\Lambda$ | `\Lambda` |
| $\mu$ | `\mu` | $\Pi$ | `\Pi` |
| $\pi$ | `\pi` | $\Sigma$ | `\Sigma` |
| $\sigma$ | `\sigma` | $\Phi$ | `\Phi` |
| $\varphi$ | `\varphi` | $\Omega$ | `\Omega` |

**运算符与关系符**

| 效果 | 源码 | 效果 | 源码 |
| --- | --- | --- | --- |
| $\pm$ | `\pm` | $\times$ | `\times` |
| $\div$ | `\div` | $\cdot$ | `\cdot` |
| $\leq$ | `\leq` | $\geq$ | `\geq` |
| $\neq$ | `\neq` | $\approx$ | `\approx` |
| $\equiv$ | `\equiv` | $\propto$ | `\propto` |
| $\in$ | `\in` | $\notin$ | `\notin` |
| $\subset$ | `\subset` | $\subseteq$ | `\subseteq` |
| $\cup$ | `\cup` | $\cap$ | `\cap` |
| $\emptyset$ | `\emptyset` | $\infty$ | `\infty` |
| $\nabla$ | `\nabla` | $\partial$ | `\partial` |

**箭头与逻辑**

| 效果 | 源码 | 效果 | 源码 |
| --- | --- | --- | --- |
| $\to$ | `\to` | $\mapsto$ | `\mapsto` |
| $\Rightarrow$ | `\Rightarrow` | $\Leftrightarrow$ | `\Leftrightarrow` |
| $\implies$ | `\implies` | $\iff$ | `\iff` |
| $\forall$ | `\forall` | $\exists$ | `\exists` |
| $\neg$ | `\neg` | $\land$ | `\land` |
| $\lor$ | `\lor` | $\therefore$ | `\therefore` |

**字体**

| 效果 | 源码 | 效果 | 源码 |
| --- | --- | --- | --- |
| $\mathbb{R}$ | `\mathbb{R}` | $\mathcal{O}$ | `\mathcal{O}` |
| $\mathfrak{g}$ | `\mathfrak{g}` | $\mathbf{w}$ | `\mathbf{w}` |
| $\mathrm{d}x$ | `\mathrm{d}x` | $\mathtt{code}$ | `\mathtt{code}` |

## 十五、公式与 Markdown 混排

粗体里的公式：**能量 $E = mc^2$ 与相对论能量 $E^2 = (pc)^2 + (mc^2)^2$ 都能正常渲染。**

列表里的行内公式：

- 勾股定理：$a^2 + b^2 = c^2$
- 欧拉公式：$e^{i\pi} + 1 = 0$
- 泰勒展开：$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x-a)^n$

引用块里放行内公式没有问题：

> 连续复利的极限：$\lim_{n \to \infty} \left(1 + \frac{r}{n}\right)^{n} = e^{r}$。

但引用块里的**独立公式**会被 Goldmark 多塞进一个 `>`，KaTeX 收到的是 `> \lim ...`，渲染只能报错。独立公式请放在引用块外，这个坑记在下一节。

## 十六、容易踩坑的写法

Goldmark 默认按 Markdown 解析公式源码，`*`、`_`、`%`、`\` 都可能被吃掉。本站开启了 `markup.goldmark.extensions.passthrough`，下面这些应当原样交给 KaTeX：

$a*b*c$、$10\%$、$\{x\}$、$x \, y$、$a \cdot b$

用表格对照一下不开 passthrough 时的后果：

| 写法 | 不开 passthrough | 开启后 |
| --- | --- | --- |
| `$a*b*c$` | 星号被当成强调，变成 `$a<em>b</em>c$` | 完整传给 KaTeX |
| `$10\%$` | 反斜杠丢失，KaTeX 收到 `$10%$` 后报错 | 正常显示百分号 |
| `$\{x\}$` | 花括号被吞，变成 `${x}$` | 正常显示集合括号 |
| `$x \, y$` | 间距命令失效 | 正常插入细间距 |
| `$a \cdot b$` | 点号命令失效 | 正常显示乘点 |

另外两个坑：

- 单个 `$` 当货币符号（例如 `$5`）会被 auto-render 当成公式起点，正文里要写美元请用“5 美元”，或者放进行内代码。
- 公式里出现 `|`（绝对值、范数）时要小心表格，`|` 会提前结束单元格。

## 十七、本站的已知限制

| 语法 | 现象 | 说明 |
| --- | --- | --- |
| `==高亮==` | 原样输出 | 未启用 `mark` 扩展 |
| 原始 HTML（`<kbd>`、`<mark>`） | 被丢弃并产生构建告警 | Goldmark 默认 `unsafe: false`，要启用得在 `hugo.yaml` 里加 `markup.goldmark.renderer.unsafe: true` |
| `\label` / `\ref` / `\eqref` | 不支持 | KaTeX 不做自动编号与交叉引用，只能用 `\tag` 手动编号 |
| `\ce{H2O}` | 不支持 | 需要额外加载 mhchem 扩展 |
| `\intertext` | 不支持 | 多行推导里不能插整段文字，拆成两个公式或用 `\text{}` |
| 公式里的中文 | 能渲染，但字形回退不可控 | 用 `\text{中文}` 包住，或把中文说明写在公式外 |
| 相邻两个 `$$` 块 | 可能被合并进同一段 | 中间插一句正文最稳妥 |
| 引用块里的独立公式 | 公式每一行前多出一个 `>` | passthrough 与 blockquote 叠加时 `>` 会留进公式，改用行内公式或移出引用块 |

KaTeX 走 jsDelivr CDN，断网或 CDN 不通时公式会退化成原始源码（页面本身不会坏）；这种情况换掉 `extend_head.html` 里的 CDN 即可。

## 十八、自检清单

| # | 检查项 | 预期 |
| --- | --- | --- |
| 1 | 页面标题 | 只出现 front matter 里的 title，没有重复的一级标题 |
| 2 | 标题锚点 | 悬停标题出现 `#`，可复制链接 |
| 3 | 目录 | TOC 按标题层级展开 |
| 4 | 粗体 / 斜体 / 删除线 | 样式生效，星号与波浪号不显示 |
| 5 | 行内代码 | 等宽字体加底色 |
| 6 | 代码块 | 语法高亮正常，超宽时可横向滚动 |
| 7 | 表格 | 三列依次为左对齐、居中、右对齐 |
| 8 | 脚注 | 上标编号可点击，文末有脚注区并可回跳 |
| 9 | 折叠块 | summary 显示自定义标题，点击展开 |
| 10 | 行内公式 | 与文字基线协调，不出现源码 |
| 11 | 独立公式 | 居中显示，aligned 的等号对齐 |
| 12 | 公式编号 | `\tag{1}` 出现在公式右侧 |
| 13 | 图片 | `static/images/rendering-test.svg` 正常加载 |
| 14 | 列表与搜索 | 新文章出现在首页列表、RSS 与搜索结果中 |

全部符合预期，就说明从 Goldmark 到 KaTeX 这条链路是通的。某一项异常时，先分清是 CDN 没加载、Hugo 或主题版本变化，还是语法本身写错——公式写错时 KaTeX 会就地显示红色错误文本，不会让整页崩掉。

本地预览用 `hugo server --buildDrafts`，本机还需要带上 `--cacheDir`，具体见仓库里的 `DEVELOPMENT.md`。
