# Cursor 知识库助手使用指南

本知识库已配置 PARA + Zettelkasten 规则与精简命令，便于在 Cursor 内完成捕获、加工与整理。

## 规则文件

- **`.cursor/rules/knowledge-base-structure.mdc`**：目录结构（PARA）、标签系统、Zettelkasten 原则、工作流程。Cursor 会自动应用。
- **`.cursor/rules/note-quality.mdc`**：笔记质量标准（原子性、链接性、标签、结构、位置）。编辑笔记时 AI 会参考。

## 可用命令（3 个）

### 1. `process-inbox-notes`
**用途**：处理收件箱中的笔记，生成结构化分析（Summary / Key Takeaway / Something New），并追加到文末。

**使用**：打开 `Inbox` 中的笔记，在 Cursor 中调用此命令或让 AI「处理这篇 Inbox 笔记」。

### 2. `extract-and-create-concepts`
**用途**：从当前摘要笔记中识别核心概念，为每个概念创建或更新原子笔记，并在原笔记中添加「相关概念」链接。一步完成识别 → 创建/更新 → 链接。

**使用**：在已分析的摘要笔记上调用，或说「从这篇笔记提取概念并创建概念笔记」。

### 3. `organize-note`
**用途**：根据 PARA 与标签规则，建议目标目录、应加标签、可建链接；必要时建议移动位置。

**使用**：对 Inbox 或未分类笔记调用，或说「整理当前笔记到合适目录并打 tag」。

## 典型工作流

1. **处理新资料**：Inbox 中的笔记 → `process-inbox-notes` →（若需沉淀概念）`extract-and-create-concepts` → `organize-note` 移动到对应 PARA 目录。
2. **检索与关联**：直接在 Cursor Chat 中提问（如「知识库中与并发相关的笔记有哪些？」），或使用 Obsidian 内 Smart Connections 做语义关联。
3. **笔记质量**：编辑后 AI 会按 `note-quality.mdc` 自动考虑原子性、链接与标签；无需单独命令。

## 提示

- 规则会自动应用；命令可通过 `@命令名` 或自然语言触发。
- 概念笔记优先放在 `Resources/Topics/` 下对应领域目录。
- 建议在仓库根目录创建 `.cursorignore`，排除 `.obsidian/`、`.trash/`、`assets/` 等，减少 AI 索引无关内容。若未创建，可手动添加上述路径。
