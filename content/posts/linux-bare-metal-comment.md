---
# 文章标题，会自动将文件名中的连字符替换为空格并首字母大写
title: '关于安装 Linux 双系统的一些提示'

# 文章创建日期，会自动填充当前日期
date: 2025-04-21T01:25:16+08:00

# 是否为草稿，true表示不会发布，false表示会发布
draft: false

# 文章标签，用于分类和搜索
tags: []

# 文章作者，可以设置单个或多个作者
author: ["Rainie Forever"]
# author: ["Me", "You"] # 多个作者的写法

# 文章描述，用于SEO和摘要
description: "Linux bare metal comment"

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

---

### **〇、安装原则**  
推荐顺序：**基础系统 → 网络 → 源 → N卡驱动 → 桌面 → 浏览器 → 字体&输入法 → 科学上网 → 安全启动 → 其他个性化**  
- 多数发行版安装时可同步完成多项配置，当试图推进下一步的时候请检查前面的步骤是否已经完成。  
- **现在是 2025 年，当你不知道下一步如何操作时，只需 ASK LLM。 所以本帖不再提供具体操作上的指导。**

---

### **一、准备工作**  
1. **备份重要数据**  

2. **检查系统信息**  
   - **UEFI**：检查是否存在 `EFI` 分区。
   - **BIOS**: 一些旧电脑不支持 UEFI，在进行下面某些步骤时请自行调整安装操作。  

3. **选择 Linux 发行版**  
   - **新手推荐**：Ubuntu LTS。  
   - 其他可选：Arch、AOSC、Fedora 等。不建议在主流硬件设备上安装 UOS 或者 deepin。  
   - 如果有频繁切换双系统的需求，建议选择默认启动项较少的发行版。  

4. **制作启动 U 盘**  
    - 文件系统：**FAT32**（确保 UEFI 兼容）。  

---

### **二、调整硬盘分区**   
   - 在 Windows 磁盘管理中，选择空闲较多的分区 `压缩卷`，生成“未分配”空间（需要 100GB+）。  
   - 无需格式化未分配空间，留待 Linux 安装程序处理。  
   - 确保未分配空间连续（碎片可能影响分区）。  

---

### **三、禁用 Windows 相关功能**  
1. **关闭快速启动**  

2. **禁用 Secure Boot**
   - 部分 Linux 发行版需要。

---

### **四、基础系统安装**  
1. **启动与安装**  
    - 插入 U 盘，从启动菜单选择 U 盘启动。然后启动安装程序。
    - 分区注意事项：  
        - **UEFI**：  
            - 若未分配空间与 Windows EFI 分区在同一磁盘，直接挂载到现有 EFI 分区（**不要新建或格式化**）。  
            - 否则新建约 300MB 的 EFI 分区。  
        - **Swap + ZRAM**：Swap 大小建议与物理内存一致。大内存不想划大磁盘空间可以开启 ZRAM。  
        - **根目录 `/`**：如果以后有重装系统的需求，单独分区，大小参考 Windows C 盘。  
        - **`/home` 目录**：剩余空间全部分配。不关心home数据也可以和根目录一个区。  
    - locale 注意事项：
        - **语言/字体/键盘 在安装桌面环境后再配置**，避免终端中文字符显示为乱码。  

---

### **五、基础配置流程**  
1. **网络配置**  
   - 校园网用户可以在启动盘预置一个 `sdunetd`，或先用手机热点完成浏览器安装。  

2. **软件源**
    - 校园网用户可选 SDU 镜像站，不过 SDU 镜像站没有镜像所有发行版的源。

2. **NVIDIA 驱动**  
    - 优先通过发行版包管理器安装闭源驱动。  
    - 次选官网下载 `.run` 文件手动安装。主流显卡直接选择最新版本即可。  
    - 重启后运行 `nvidia-smi` 确认驱动状态。笔记本用户若有显示 `N/A`，属正常现象。   
    - 安装 PyTorch 并运行 `torch.cuda.is_available()` 测试 CUDA。  
    - 安装 Miniconda 后，如果不喜欢 `.bashrc` 的改动，可额外安装一个其他的 Shell 设为默认。  

---

### **六、安装后处理**  
1. **网络工具**  
   - **GitHub 直连**：  
     - 优先配置 SSH Key，必要时使用科学上网。  

2. **安全启动**  
   - 若需重新启用 Secure Boot：  
     - Ubuntu 支持较好，安装后直接启用。  
     - Arch 需手动签名内核。  
     - 其他发行版请自行查阅资料。

---

