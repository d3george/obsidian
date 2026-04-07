# Slash-Space 知识库

本知识库使用 PARA + Zettelkasten 方法组织。以下规则在所有交互中自动应用。

## 用户画像

- **角色：** TypeScript 全栈开发工程师，10 年经验
- **当前主职：** 远程前端（Vue3）
- **技术栈默认：** 前端优先 **TanStack Start**（Router、Query、Table、Form），后端 **Hono + Bun + PostgreSQL**
- **2026 计划：** `Inbox/2026-ToDo.md` 为唯一事实来源

## 目录结构 (PARA)

- `Inbox/` - 临时笔记，需定期清理
- `Projects/` - 有明确截止日期的工作
- `Areas/` - 长期负责的领域
- `Resources/` - 个人知识库
  - `Topics/[领域]/` - 概念驱动的原子笔记
  - `Sources/{Books,Videos,Articles}/` - 来源摘要
- `Archive/` - 已完成/过时的内容

## 标签系统

四类命名空间：`type/`、`status/`、`source/`、`topic/`。frontmatter 中使用 YAML 列表，值不加引号。

### type/（笔记类型，闭集）
`type/concept` `type/summary` `type/snippet` `type/cheatsheet` `type/meeting` `type/question` `type/insight` `type/prompt`

### status/（工作流状态，闭集）
`status/todo` `status/in-progress` `status/waiting` `status/done` `status/review`

### source/（信息来源，闭集）
`source/book` `source/article` `source/video` `source/podcast` `source/course` `source/conversation`

### topic/（跨领域主题，单层扁平）
仅使用 `topic/xxx` 单层，禁止嵌套。推荐：api, system-design, career, finance, writing, ai, devops, git, obsidian, tools, web-development, microservices, enterprise, websocket, ui。多词用小写+连字符。

### 标签使用
- 每篇至少 1 个 type；摘要类必有 1 个 source；主题 1–3 个 topic
- 概念笔记：type/concept + 1–3 个 topic
- 摘要笔记：type/summary + source/xxx + 0–1 个 status + 1–3 个 topic
- 不重复目录名作为标签

## Zettelkasten 原则

1. **概念驱动**：以概念为核心，而非来源
2. **原子化**：每个概念一个笔记
3. **双向链接**：使用 `[[笔记名]]`
4. **来源追溯**：概念笔记链接回原始来源

## 笔记质量标准

编辑或创建笔记时自动考虑：

1. **原子性**：一笔记一概念，多个概念应建议拆分
2. **链接性**：概念笔记含「相关概念」与「来源」；摘要笔记链接到概念笔记
3. **标签**：遵循标签系统，补全缺失项
4. **结构**：概念笔记含「定义」「核心要点」「应用场景」「相关概念」「来源」
5. **位置**：概念→Resources/Topics/[领域]/；摘要→Resources/Sources/[类型]/；项目→Projects/[项目名]/；领域→Areas/[领域]/

## 文风约定

- 简体中文；句子简短，一段一个意思
- 客观、信息优先；避免夸张/营销式表述
- 术语一致；专有名词首次出现可简短解释
- 不强行统一既有笔记的原有文风

## 典型工作流

1. **捕获** → Inbox（快速记录）
2. **加工** → `process-inbox-notes` 分析 → `extract-and-create-concepts` 沉淀概念
3. **整理** → `organize-note` 移动到 PARA 目录 + 打标签
4. **关联** → 双向链接连接相关笔记
