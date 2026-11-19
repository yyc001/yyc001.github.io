# 开发指南

> 这份备忘的目标：换一台机器 / 隔半年回来，也能在 5 分钟内把站点跑起来，并且避开已经踩过的坑。
>
> 记录时的环境：`Hugo 0.170.1 (extended)` + `PaperMod 1181196` · Windows + PowerShell · 2026-11-19

---

## 1. 项目概览

静态博客，Hugo 生成，PaperMod 主题，部署在 GitHub Pages。

| 项目 | 值 |
| --- | --- |
| 站点地址 | <https://yyc001.github.io/> |
| 开发分支 | `source` |
| 发布分支 | `gh-pages`（CI 自动推送，**不要手动改**） |
| 主题 | PaperMod（git submodule，固定在 `1181196`） |
| 构建产物 | `public/`（已 gitignore，不提交） |

### 目录结构

```
yyc001.github.io/
├── .github/workflows/deploy.yml   # CI：推 source → 构建 → 推 gh-pages
├── archetypes/default.md          # 新文章模板（front matter 就来自这里）
├── content/
│   ├── posts/                     # 文章，URL = /posts/<文件名>/
│   └── story/                     # 另一个 section，URL = /story/<文件名>/
├── layouts/_partials/
│   └── extend_head.html           # 站点级覆盖：按需注入 KaTeX
├── themes/PaperMod/               # 主题（submodule，只读，别直接改）
├── .tools/                        # 本地工具链，gitignore 掉
│   ├── hugo/hugo.exe              # Hugo 没有装进系统 PATH
│   └── hugo-cache/                # Hugo 缓存（沙箱要求放这里）
├── hugo.yaml                      # 唯一配置：baseURL / 标题 / markdown 公式透传
└── public/                        # 构建输出，gitignore
```

---

## 2. 环境准备

### 2.1 Hugo（必须是 extended 版）

CI 用的是 `extended: true`，本地请保持一致，否则 SCSS / 图片处理会报错。

本机装法是**便携式**的，没有进 PATH：

```powershell
.tools\hugo\hugo.exe version
# hugo v0.170.1-... windows/amd64 BuildDate=... VendorInfo=gohugoio
```

如果 `.tools/` 不在了（新机器 / 被清理），重新下载：

```powershell
# 版本号换成当前最新：https://github.com/gohugoio/hugo/releases
$ver = '0.170.1'
$url = "https://github.com/gohugoio/hugo/releases/download/v$ver/hugo_extended_${ver}_windows-amd64.zip"
# 用 Node 下载（见 §7 坑 7：PowerShell 自带的下载在这里会失败）
node -e "const fs=require('fs');fetch(process.argv[1]).then(r=>r.arrayBuffer()).then(b=>fs.writeFileSync('hugo.zip',Buffer.from(b)))" $url
Expand-Archive hugo.zip -DestinationPath .tools\hugo -Force
```

> 想偷懒装系统级也可以：`winget install Hugo.Hugo.Extended`。装完 `hugo` 直接在 PATH 里，下面命令里的 `.\.tools\hugo\hugo.exe` 换成 `hugo` 即可。

### 2.2 主题子模块

主题是 **submodule**，克隆主仓库后 `themes/PaperMod/` 是空目录，必须初始化：

```powershell
git submodule update --init --recursive
```

⚠️ **在 DSH 沙箱里这条会失败**（见 §7 坑 3）。替代做法：

```powershell
git clone https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git -C themes/PaperMod fetch --depth 1 origin <gitlink记录的commit>
git -C themes/PaperMod checkout <gitlink记录的commit>
```

查 gitlink 记录的 commit：

```powershell
git ls-tree HEAD themes/PaperMod
```

---

## 3. 本地开发

### 起预览服务器

```powershell
cd C:\Users\yyc001\Desktop\dashaohuo\yyc001.github.io
& .\.tools\hugo\hugo.exe server --buildDrafts --cacheDir "$PWD\.tools\hugo-cache"
# 打开 http://localhost:1313
```

- `--buildDrafts` 让你能看到 `draft: true` 的文章（**不加就看不到草稿**）
- `--cacheDir` 是必须的，原因见 §7 坑 1
- 改文件自动热更新，Ctrl+C 退出

### 生成静态文件

```powershell
& .\.tools\hugo\hugo.exe --minify --destination ./public --cleanDestinationDir --cacheDir "$PWD\.tools\hugo-cache"
```

`--minify` / `--destination ./public` 与 CI 保持一致，这样本地看到的就是线上效果。产物在 `public/`，已被 gitignore。

---

## 4. 写文章

### 新建

```powershell
& .\.tools\hugo\hugo.exe new content posts\my-new-post.md
# 或放进 story section
& .\.tools\hugo\hugo.exe new content story\my-piece.md
```

会套用 `archetypes/default.md`，自动填标题（文件名连字符转空格）、日期、`draft: true`。

### 改这三个地方就能发

```yaml
title: '中文标题写这里'    # 文件名只管 URL，标题不受它限制
draft: false              # ← 忘了改这句，文章就永远不会上线
description: '一句话摘要'  # 用于列表页摘要和 SEO
```

### 容易被坑的规则

| 规则 | 说明 |
| --- | --- |
| **文件名 = URL** | `content/posts/my-new-post.md` → `/posts/my-new-post/`。想用中文标题但英文链接，就英文文件名 + 中文 `title`；或加 `slug: 自定义` |
| **`_` 开头的文件被忽略** | `_draft.md` 不会生成页面（Hugo 对下划线开头的文件不出页），调试时会被误认为"构建失败" |
| **未来日期不发布** | `date` 在未来的文章默认不生成，除非加 `--buildFuture` |
| **`draft: true` 不发布** | 构建时要 `--buildDrafts` 才可见 |
| **`weight: 1`** | 模板里每个新文章权重都是 1；PaperMod 列表默认按日期倒序，需要置顶再调这个值 |
| **图片** | 放 `static/images/`，正文写 `![](/images/pic.png)` |

### front matter 参考

完整字段（含所有注释）看 `archetypes/default.md`。常用几个：

```yaml
title: ''
date: 2026-11-19T15:42:24+08:00
draft: false
tags: ["标签1", "标签2"]
description: ''
showToc: true        # 显示目录
TocOpen: true        # 目录默认展开
math: true           # 启用数学公式（见 §5）
cover:               # 封面图
  image: /images/cover.jpg
  alt: ''
```

> 模板有 80+ 行注释，每篇新文章都会带一份。嫌吵可以精简 `archetypes/default.md`，只留常用字段。

---

## 5. 数学公式（LaTeX / KaTeX）

Hugo **不内置** LaTeX 渲染，本项目用「Goldmark passthrough + KaTeX」两段式实现。

### 用法

1. 文章的 front matter 加一行：`math: true`
2. 正文正常写：

```markdown
行内公式 $E = mc^2$

$$
\int_{0}^{1} x^{2} \, \mathrm{d}x = \frac{1}{3}
$$
```

### 原理

| 层 | 位置 | 作用 |
| --- | --- | --- |
| 后端 | `hugo.yaml` 的 `markup.goldmark.extensions.passthrough` | 让 `$...$` / `$$...$$` 里的内容**原样透传**，不被 Markdown 当成语法解析 |
| 前端 | `layouts/_partials/extend_head.html` | 只在 `math: true` 的页面注入 KaTeX 0.19.1（jsDelivr CDN），由 auto-render 扫描页面渲染 |

### 为什么必须开 passthrough

不开的话 Goldmark 会破坏公式源码。实测对照：

| 写法 | 不开 passthrough | 开启后 |
| --- | --- | --- |
| `$a*b*c$` | `$a<em>b</em>c$` ❌ | 原样 ✅ |
| `$10\%$` | `$10%$` ❌ | 原样 ✅ |
| `$\{x\}$` | `${x}$` ❌ | 原样 ✅ |
| `$a\\b$` | `$a\b$` ❌ | 原样 ✅ |

### 注意

- **KaTeX 的 auto-render 默认不认单个 `$`**，`extend_head.html` 里已显式声明 delimiters，别删。
- 代码块（三个反引号围起来的区域）里的 `$` 不受影响，KaTeX 会跳过 `pre` / `code`。
- KaTeX 走 CDN，**断网或 CDN 不通时公式会退化成原始源码**，正文不会坏。国内网络若加载失败，把 `extend_head.html` 里的 `cdn.jsdelivr.net` 换成 `npm.elemecdn.com` 或 `cdn.bootcdn.net`。
- 想换 MathJax（支持 `\label` / `\ref` / 化学式等更全宏）只改 `extend_head.html`，正文写法不变。
- 示例文章：`content/posts/math-formula-demo.md`。

---

## 6. 部署

```powershell
git add .
git commit -m "post: xxx"
git push origin source
```

推送到 `source` 后 `.github/workflows/deploy.yml` 自动执行：

1. `actions/checkout@v2` + `submodules: recursive` → 拉代码和主题
2. `peaceiris/actions-hugo@v2`，`hugo-version: latest`，`extended: true`
3. `hugo --minify --destination ./public`
4. `peaceiris/actions-gh-pages@v3` → 推 `public/` 到 `gh-pages`

**不需要**本地构建后再提交 `public/`。

### 风险点

CI 用 `hugo-version: 'latest'`，意味着 **Hugo 每次发版都可能悄悄改变构建行为**。历史上已经因此炸过一次（Hugo 0.156.0 移除 `site.Author` 导致主题的 `rss.xml` 报错，见 §7 坑 4）。如果哪天推送后 Actions 失败，优先怀疑这个。想彻底稳住，可以把 workflow 里的 `latest` 改成固定版本号（例如 `'0.170.1'`），并和本地保持一致。

---

## 7. 已踩过的坑

### 坑 1：`mkdir ...\Temp\hugo_cache: Access is denied`

**现象**：`hugo` / `hugo server` 启动即失败。

**原因**：Hugo 启动时要建缓存目录，默认在 `%TEMP%\hugo_cache`。DSH 的 `workspace-write` 沙箱只允许写工作区和它分配的临时子目录，系统 Temp 根目录在名单外。

**解决**：把缓存指到工作区内（**必须是绝对路径**）：

```powershell
--cacheDir "$PWD\.tools\hugo-cache"
```

或设环境变量 `HUGO_CACHEDIR`。

> 注意：**不要**把 `cacheDir` 写进 `hugo.yaml` —— Hugo 要求绝对路径，写进去就变成机器专属路径，CI 会报错。

### 坑 2：`".tools\hugo-cache\modules" must resolve to an absolute directory`

`--cacheDir` 传了相对路径。改用 `"$PWD\.tools\hugo-cache"` 展开成绝对路径。

### 坑 3：`git submodule update --init` 报 `couldn't create signal pipe, Win32 error 5`

**原因**：DSH 沙箱禁止程序创建命名管道，而 `git submodule` 会调 `sh.exe`，MSYS 运行时需要信号管道。

**解决**：改用 `git clone` 手动拉主题（§2.2）。日常构建不需要 submodule 命令，影响有限。

### 坑 4：`can't evaluate field Author in type *page.siteWrapper`

**原因**：主题旧版本 `layouts/_default/rss.xml` 里用了 `site.Author`，该方法在 **Hugo v0.156.0 被移除**。

**解决**：主题已升级到 `1181196`（该版本删掉了自带的 `rss.xml`，改用 Hugo 内置模板）。若回退主题版本会重现。

### 坑 5：文章构建成功但在站点上找不到

按顺序排查：

1. `draft: true`？→ 需要 `--buildDrafts`，发布要改成 `false`
2. `date` 是未来时间？→ 需要 `--buildFuture`
3. 文件名以 `_` 开头？→ Hugo 直接忽略该文件
4. 文件没放在 `content/` 下？

### 坑 6：`languageCode` 弃用警告

```
WARN deprecated: project config key languageCode was deprecated in Hugo v0.158.0
```

只是警告，不影响构建。将来被移除时把 `hugo.yaml` 里的 `languageCode: zh-cn` 换成 `locale: zh-CN`。

### 坑 7：PowerShell `Invoke-RestMethod` / `curl.exe` 下载 GitHub 失败

**现象**：`基础连接已经关闭` 或 `schannel: AcquireCredentialsHandle failed`。

**原因**：沙箱环境下 .NET / SChannel 的 TLS 栈拿不到可用凭证。

**解决**：用 Node（自带 OpenSSL）：

```powershell
node -e "const fs=require('fs');fetch(process.argv[1]).then(r=>r.arrayBuffer()).then(b=>fs.writeFileSync('out.zip',Buffer.from(b)))" <url>
```

`git clone` / `git fetch` 也正常，下载 Git 仓库优先用 git。

### 坑 8：`LF will be replaced by CRLF`

仓库的 `core.autocrlf` 归一化行为，不影响文件内容，忽略即可。

---

## 8. 维护清单

### 升级主题

```powershell
git -C themes/PaperMod fetch --depth 1 origin master
git -C themes/PaperMod checkout master
git add themes/PaperMod          # 记录新的 gitlink
& .\.tools\hugo\hugo.exe --minify --destination ./public --cacheDir "$PWD\.tools\hugo-cache"   # 必须本地验证通过
```

升级后务必跑一次构建 —— 主题和 Hugo 版本强耦合，很容易炸在模板上（见坑 4）。

### 升级 Hugo

改 `.tools/hugo/` 下的二进制即可，同样要在本地构建验证一遍。

### 已知遗留问题

- `.hugo_build.lock` 被误提交进仓库（Hugo 的临时锁文件，0 字节）。可以 `git rm --cached .hugo_build.lock` 并加进 `.gitignore`。
- `archetypes/default.md` 注释过多，每篇新文章都带 80+ 行注释，可精简。
- `themes/PaperMod/.git` 是目录（手动 clone 的产物）而非 submodule 标准的 gitfile，不影响构建和 CI。

---

## 9. 命令速查

```powershell
$hugo = ".\.tools\hugo\hugo.exe"
$cache = "$PWD\.tools\hugo-cache"

& $hugo server --buildDrafts --cacheDir $cache                  # 本地预览
& $hugo --minify --destination ./public --cacheDir $cache       # 生产构建
& $hugo new content posts\new-post.md                           # 新建文章
& $hugo version                                                 # 查版本

git ls-tree HEAD themes/PaperMod                                # 看主题固定版本
git submodule status                                            # 看主题状态（沙箱内会失败）
git push origin source                                          # 触发部署
```
