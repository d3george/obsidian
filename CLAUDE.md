# Slash-Space 知识库

本知识库采用 **[LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)** 模式。LLM 自动维护 wiki 层，人类负责信息来源和方向引导。

核心理念：不做 RAG（每次重新检索），而是 **持续编译一个持久的 wiki**。知识编译一次，持续保持最新。每摄入一个新来源，可能更新 10-15 个 wiki 页面。

## 用户画像

- **角色：** TypeScript 全栈开发工程师，10 年经验
- **当前主职：** 远程前端（Vue3）
- **技术栈默认：** 前端优先 **TanStack Start**（Router、Query、Table、Form），后端 **Hono + Bun + PostgreSQL**

## 三层架构

1. **Raw（原始来源）** — `raw/` 目录，不可变。LLM 只读不写。事实来源。
2. **Wiki（知识层）** — `wiki/` 目录，LLM 拥有。LLM 写入维护，人类阅读浏览。
3. **Schema（规则）** — 本文件，与 LLM 共同演进。

## 目录结构

```
slash-space/
├── raw/                      # 层 1：原始来源（不可变，LLM 只读）
│   ├── Articles/
│   └── assets/               # 所有附件
├── wiki/                     # 层 2：LLM 维护的知识库
│   ├── index.md              # 内容目录
│   ├── log.md                # 操作日志（append-only）
│   ├── concepts/             # 概念页
│   ├── sources/              # 来源摘要页
│   ├── comparisons/          # 对比页
│   └── synthesis/            # 综合页
└── CLAUDE.md                 # 层 3：Schema
```

> 需要时可在 `raw/` 下新建 `Books/`、`Videos/`、`Podcasts/` 等子目录。可在 `wiki/concepts/` 下新建主题子目录。

## Wiki 页面格式

所有 wiki 页面使用 YAML frontmatter + markdown 正文。frontmatter 至少包含 `created` 和 `updated` 日期。

### 概念页（`wiki/concepts/[topic]/[概念名].md`）

每个概念一个页面。包含：定义、核心要点、应用场景、相关概念（`[[链接]]`）、来源（`[[链接]]`）。

```markdown
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: N
---

# 概念名

## 定义
一句话定义

## 核心要点
- ...

## 相关概念
- [[概念A]]

## 来源
- [[来源摘要]]
```

### 来源摘要页（`wiki/sources/[来源名].md`）

从 raw 编译生成的摘要。包含：摘要、核心要点、值得关注、提取的概念。

```markdown
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
title: 原标题
author: 作者
url: 原始链接
raw: "[[raw 中的文件名]]"
---

# 来源名

## 摘要
2-4 句话概括

## 核心要点
- ...

## 提取的概念
- [[概念A]]
- [[概念B]]
```

### 对比页 / 综合页

对比页：多个概念的多维度对比表格。
综合页：跨来源的主题综合分析。

## 操作流程

### Ingest（摄入）
触发：`/ingest [来源文件]`

1. 读取 `raw/` 中的来源
2. 与用户讨论关键要点（可选）
3. 在 `wiki/sources/` 创建来源摘要页
4. 识别概念 → 在 `wiki/concepts/` 创建或更新概念页
5. 更新已有页面中的交叉引用和矛盾标记
6. 更新 `wiki/index.md`
7. 追加条目到 `wiki/log.md`

一个来源可能涉及 10-15 个 wiki 页面的更新。

### Query（查询）
触发：`/query [问题]`

1. 读取 `wiki/index.md` 定位相关页面
2. 钻入具体页面读取详情
3. 综合回答，附 `[[引用]]`
4. 有价值的回答沉淀为新的对比页或综合页
5. 追加条目到 `wiki/log.md`

### Lint（检查）
触发：`/lint`

1. 扫描 `wiki/` 全部页面
2. 检查：孤立页、死链接、缺失概念、矛盾、frontmatter 不完整
3. 生成报告和修复建议
4. 追加条目到 `wiki/log.md`

## 导航文件

### wiki/index.md
- 每次 ingest / query / lint 后更新
- 分类列出所有页面，每个页面附一行摘要
- 底部统计数据

### wiki/log.md
- Append-only，永不删除或修改已有条目
- 格式：`## [YYYY-MM-DD] 操作类型 | 描述`

## 质量标准

1. **交叉引用**：概念页链接到相关概念和来源；来源页链接到提取的概念
2. **矛盾标记**：新来源与旧内容矛盾时，不静默覆盖，用 `[!contradiction]` 标注
3. **frontmatter**：包含 `created`、`updated`；概念页含 `sources` 计数
4. **图片**：统一存放在 `raw/assets/`，用标准 markdown `![](path)` 引用

## 文风约定

- 简体中文，句子简短
- 客观、信息优先
- 术语一致，首次出现可简短解释
