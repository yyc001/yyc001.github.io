---
# 文章标题，会自动将文件名中的连字符替换为空格并首字母大写
title: '生成式 AI 与软件工程'

# 文章创建日期，会自动填充当前日期
date: 2026-11-19T18:10:07+08:00

# 是否为草稿，true表示不会发布，false表示会发布
draft: false

# 文章标签，用于分类和搜索
tags: ["AI", "软件工程", "工作流"]

# 文章作者，可以设置单个或多个作者
author: ["Rainie Forever"]

# 文章描述，用于SEO和摘要
description: '生成式 AI '

# 文章权重，用于排序，数值越大越靠前
weight: 1

# 编辑链接设置
editPost:
    URL: "https://github.com/yyc001/yyc001.github.io/blob/source/content/" # 编辑页面的基础URL
    Text: "View Source" # 编辑按钮的文本
    appendFilePath: true # 是否在URL后追加文件路径

# 显示控制
showToc: true        # 是否显示目录
TocOpen: true        # 目录是否默认展开
hidemeta: false      # 是否隐藏元数据
comments: true       # 是否启用评论
ShowReadingTime: true
ShowWordCount: true
ShowPostNavLinks: true
UseHugoToc: true

# 文章最后修改时间
# lastmod: ""
---

### Control of Source Code

> 未来人类一定会放弃逐行 reivew 源代码

为什么？

你总是需要这样的代码：

```cpp
/**
* @prompt 请生成一个极致优化后的汉诺塔
*/
void hanoi(unsigned n) {
    const std::uint64_t total = (std::uint64_t{1} << n) - 1; 
    const unsigned dir1 = (n & 1u) ? 2u : 1u;
    for (std::uint64_t k = 1; k <= total; ++k) {
        const unsigned d    = unsigned(__builtin_ctzll(k)) + 1;
        const unsigned step = (d & 1u) ? dir1 : 3u - dir1;
        const unsigned from = unsigned((k >> d) % 3u) * step % 3u;
        emit(from, (from + step) % 3u);
    }
}
```

它是对的，但是一般开发者看不懂。  
这个时候只有 prompt 是可以 review 的。  
我们必须接受这种事情发生。

所以怎么办？目前还没有 golden practice，我的观点：

> 1. intent > code
>    - 无论是否放弃掌握代码，都要把原始意图留下来。
> 2. 约束和测试会越来越重要
>    - Unit / Regression / Fuzz / Confirmation / Difftest 
>    - Lint



### Version Control

Git 对 AI agent 为主体的开发模式并不友好，已经成为业界的共识。

> - commit 不再 reviewable
> - agent 多开怎么办？并发灾难。不建 worktree 马上炸。
> - User->Agent->User->Agent 改了多轮，何时commit？message怎么写？
> - 对比测试怎么办？n 个 branch 删掉 n-1 个？
> - CI 爆炸，`git bisect` 失灵

以及最重要的，
> - 原始 intent 丢失


一些目前面向agent的版本控制工具的尝试：

[Jujutsu(jj)](https://www.jj-vcs.dev/)
: 兼容 Git。抛弃 `git add` `stash`，让 `commit` 修改常态化 

Scion
: [Anthropic 26年10月份的post](https://www.anthropic.com/research/redesigning-source-control)，抛弃了 Git 自建的版本控制软件。基本不具备通用性

### Spec-driven Development

因为代码是golden truth，doc是代码在一个方向上的投影
sdd到最后你一定不会验doc的，这里就有语义漂移
我现在搞大项目，做到后面就把doc删了，缩成随代码附赠的可数个可独立验证的自然语言断言
因为doc随时可以regenerate，这些断言才反映你本来的开发意图


### Overfitting

人和 AI 并没有本质上的不同

Agent 表现出的奇怪行为，过拟合的结果。

人类一样会过拟合。

人类在学校和社会上被规训了数十年，训练一个标准化执行机器。

但这是 领导/老师/社会默认 分配的工作，也要在这个错误的方向上执行下去。



## 结语

人类
