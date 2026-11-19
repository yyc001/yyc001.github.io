---
# 文章标题，会自动将文件名中的连字符替换为空格并首字母大写
title: '数学公式排版示例'

# 文章创建日期，会自动填充当前日期
date: 2026-11-19T15:42:24+08:00

# 是否为草稿，true表示不会发布，false表示会发布
draft: false

# 是否启用本页的数学公式渲染（加载 KaTeX），关闭时不会额外加载任何资源
math: true

# 文章标签，用于分类和搜索
tags: ["LaTeX", "Markdown"]

# 文章作者，可以设置单个或多个作者
author: ["Rainie Forever"]
# author: ["Me", "You"] # 多个作者的写法

# 文章描述，用于SEO和摘要
description: '在 Hugo 里渲染 LaTeX 数学公式的完整示例：行内公式、独立公式与常见坑'

# 文章权重，用于排序，数值越大越靠前
weight: 1

# 文章别名，可以设置多个URL指向同一篇文章
# aliases: ["/first"]

# 文章封面图片设置
# cover:
#     image: "/images/cover.jpg" # 图片路径或URL
#     alt: "封面图片" # 图片替代文本
#     caption: "图片说明" # 图片说明文字
#     relative: false # 使用页面包时设置为true
#     hidden: true # 是否只在当前页面隐藏

# 编辑链接设置
editPost:
    URL: "https://github.com/yyc001/yyc001.github.io/blob/source/content/" # 编辑页面的基础URL
    Text: "View Source" # 编辑按钮的文本
    appendFilePath: true # 是否在URL后追加文件路径

# 显示控制
# 是否显示目录
showToc: true

# 目录是否默认展开
TocOpen: true

# 是否隐藏元数据（如作者、日期等）
hidemeta: false

# 是否启用评论功能
comments: true

# 是否禁用分享按钮
disableShare: false

# 是否禁用代码高亮
disableHLJS: false

# 是否隐藏文章摘要
hideSummary: false

# 是否在搜索结果中隐藏
searchHidden: false

# 是否显示阅读时间
ShowReadingTime: true

# 是否显示面包屑导航
ShowBreadCrumbs: true

# 是否显示上一篇/下一篇文章链接
ShowPostNavLinks: true

# 是否显示字数统计
ShowWordCount: true

# 是否在分类页面显示RSS按钮
ShowRssButtonInSectionTermList: true

# 是否使用Hugo内置的目录生成功能
UseHugoToc: true

# 规范URL，用于SEO
# canonicalURL: "https://yyc001.github.io/"

# 文章最后修改时间
# lastmod: ""
---

# 数学公式排版示例

这篇文章用来验证公式渲染是否正常。front matter 里的 `math: true` 会让本页额外加载 KaTeX。

## 行内公式

质能方程 $E = mc^2$ 写在句子中间。

带下标和上标的写法：$a_i + b_i^2$、$\sum_{i=1}^{n} i = \frac{n(n+1)}{2}$。

容易出错的几个字符，在开启公式支持后应当完整保留：
$a * b * c$、$10\%$、$\{x\}$、$a \\ b$、$x \, y$。

## 独立公式

$$
\int_{0}^{1} x^{2} \, \mathrm{d}x = \frac{1}{3}
$$

多行对齐：

$$
\begin{aligned}
(a+b)^2 &= a^2 + 2ab + b^2 \\
(a-b)^2 &= a^2 - 2ab + b^2
\end{aligned}
$$

矩阵：

$$
A = \begin{pmatrix}
a_{11} & a_{12} \\
a_{21} & a_{22}
\end{pmatrix}
$$

## 希腊字母与常见符号

$\alpha$、$\beta$、$\gamma$、$\theta$、$\lambda$、$\mu$、$\sigma$、$\omega$

$\infty$、$\partial$、$\nabla$、$\forall$、$\exists$、$\in$、$\subseteq$、$\Rightarrow$、$\approx$、$\leq$、$\geq$、$\neq$

## 代码块里的 $ 不受影响

公式渲染会跳过代码块，所以下面这些内容会原样显示：

```bash
echo $HOME && awk '{print $1}' file.txt
```

```latex
% 在代码块里写 LaTeX 源码也不会被 KaTeX 处理
\frac{a}{b}
```

## 写作提示

| 想要的效果 | 写法 |
| --- | --- |
| 行内公式 | `$...$` |
| 独立居中公式 | `$$...$$` 单独成段 |
| 多行对齐 | `\begin{aligned}...\end{aligned}`，用 `&` 对齐、`\\` 换行 |
| 矩阵 | `\begin{pmatrix}...\end{pmatrix}` |

想换成 MathJax（支持 `\label`、`\ref`、化学式等更全的宏）时，只需替换 `layouts/_partials/extend_head.html` 里的 CDN 与初始化代码，正文写法不用改。
