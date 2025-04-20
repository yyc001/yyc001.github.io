---
# 文章标题，会自动将文件名中的连字符替换为空格并首字母大写
title: '{{ replace .Name "-" " " | title }}'

# 文章创建日期，会自动填充当前日期
date: {{ .Date }}

# 是否为草稿，true表示不会发布，false表示会发布
draft: true

# 文章标签，用于分类和搜索
tags: []

# 文章作者，可以设置单个或多个作者
author: ["Rainie Forever"]
# author: ["Me", "You"] # 多个作者的写法

# 文章描述，用于SEO和摘要
description: '{{ replace .Name "-" " " | title }}'

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

# 文章标题

## 副标题

> 引用内容

正文内容

## 章节标题

### 子标题

> 引用内容

正文内容
