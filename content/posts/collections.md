---
# 文章标题，会自动将文件名中的连字符替换为空格并首字母大写
title: '阅读笔记：[DAC25]MCH'

# 文章创建日期，会自动填充当前日期
date: 2025-07-08T09:23:00+08:00

# 是否为草稿，true表示不会发布，false表示会发布
draft: true

# 文章标签，用于分类和搜索
tags: []

# 文章作者，可以设置单个或多个作者
author: ["Rainie Forever"]
# author: ["Me", "You"] # 多个作者的写法

# 文章描述，用于SEO和摘要
description: "[DAC'25] Mixed Structural Choice Operator: Enhancing Technology Mapping with Heterogeneous Representations"

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
lastmod: 2025-07-11T15:57:00+08:00
---

# Mixed Structural Choice Operator: Enhancing Technology Mapping with Heterogeneous Representations
# 混合结构选择算子：用异构表示增强工艺映射

## preliminaries

MFFC

### MCH

input AIG (只有一个输出？)

1to1 mapping -> MIG (每个节点都加一个0输入即可)

*critical path collection

for node:

    if node in critical path:
        enumerate [node, cut]:
            level-oriented systhesis for cut
    
    else:
        enumerate [node, cut]:
            area-oriented systhesis for cut

        area-oriented systhesis for MFFC


How to calculate critical path?

optimization methods?

level oriented
- 4-input NPN library (Negation Permutation Negation)
area oriented
- SOP sum of products
- DSD?? disjoint support decomposition???

save choices?
- node hash?

### technology mapping

for cut in cut(n) + cuts(n'):
    select cut mapping?

### Logic optimization 


## experiments

Mockturtle framework

XAG XMG

## 质疑


