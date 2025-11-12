# Cursor 知识库助手使用指南

本知识库已配置了 PARA + Zettelkasten 方法的规则和命令，帮助您更高效地管理和检索笔记。

## 📋 规则文件

### `.cursor/rules/knowledge-base-structure.md`
定义了知识库的组织结构、标签系统和 Zettelkasten 原则。Cursor 会自动应用这些规则来理解您的知识库。

## 🛠️ 可用命令

### 1. `process-inbox-notes`
**用途**：处理收件箱中的笔记，生成结构化分析

**使用方法**：
- 打开 `Inbox` 中的笔记
- 在 Cursor 中使用此命令
- 会自动生成摘要、要点和新见解

### 2. `create-concept-note`
**用途**：创建原子化的概念笔记

**使用场景**：
- 学习到新概念需要记录
- 从来源笔记中提炼概念

**示例**：
```
@create-concept-note 概念名：Goroutines，领域：GoLang，描述：Go 语言的轻量级线程
```

### 3. `extract-concepts-from-note`
**用途**：从摘要笔记中提取概念并创建独立笔记

**使用场景**：
- 有一篇读书笔记或视频总结
- 需要将其中的概念提炼成独立笔记

**示例**：
```
@extract-concepts-from-note 从当前笔记中提取所有概念
```

### 4. `find-related-notes`
**用途**：查找相关笔记并建议链接关系

**使用场景**：
- 想要发现知识库中的关联
- 建立笔记之间的连接

**示例**：
```
@find-related-notes 查找与"并发"相关的所有笔记
```

### 5. `organize-note`
**用途**：整理笔记到正确位置并添加标签

**使用场景**：
- 清理 Inbox
- 整理未分类的笔记

**示例**：
```
@organize-note 整理当前笔记到正确的目录
```

### 6. `search-knowledge-base`
**用途**：在知识库中搜索并回答问题

**使用场景**：
- 查找特定主题的信息
- 理解概念之间的关系

**示例**：
```
@search-knowledge-base 什么是 Raft 算法？知识库中有哪些相关内容？
```

### 7. `suggest-note-improvements`
**用途**：分析笔记并提出改进建议

**使用场景**：
- 优化现有笔记
- 提高笔记质量

**示例**：
```
@suggest-note-improvements 分析当前笔记并提出改进建议
```

## 🚀 典型工作流

### 场景 1：处理新学习的视频
1. 在 `Inbox` 快速记录
2. 使用 `process-inbox-notes` 生成分析
3. 使用 `extract-concepts-from-note` 提取概念
4. 使用 `organize-note` 移动到 `Resources/Sources/Videos/`

### 场景 2：创建新概念笔记
1. 使用 `create-concept-note` 创建笔记
2. 使用 `find-related-notes` 查找相关笔记
3. 建立双向链接
4. 使用 `suggest-note-improvements` 优化笔记

### 场景 3：检索知识
1. 使用 `search-knowledge-base` 提问
2. Cursor 会在整个知识库中搜索
3. 返回相关笔记和链接建议

## 💡 提示

- 规则文件会自动应用，无需手动调用
- 命令可以通过 `@命令名` 的方式在 Cursor 中使用
- 所有命令都理解 PARA 和 Zettelkasten 的结构
- 优先在 `Resources/Topics/` 中查找概念笔记
