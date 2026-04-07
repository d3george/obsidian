---
tags:
  - type/concept
  - topic/ai
  - source/article
  - status/done
---

# AI MCP (Model Context Protocol)

## 定义

**一句话：** MCP（Model Context Protocol）是 AI 模型的**「通用转接头协议」**——让 AI 能安全、标准化地连接外部工具、数据库、API 和文件系统，解决「每个工具都要单独对接」的碎片化问题。

**比喻：** 像**出国旅行的万能转换插头**：以前去不同国家要带一堆专属插头（英标、欧标、美标），现在一个 USB-C 转接头就能插遍全球插座。MCP 让 AI 用同一套「接口语言」对接 Google Drive、数据库、Slack、浏览器……插上去就能通信。

> **对应关系**：「万能插头」对应**标准化的协议格式**；「各国插座」对应**不同的外部系统**（数据库、API、本地文件）；「USB-C 端」对应**AI 模型统一调用**；「转接头内部电路」对应**MCP 服务器的适配逻辑**（认证、数据转换、安全沙箱）。不较真之处：真实 MCP 服务器可能需要配置密钥或 OAuth，不像物理插头即插即用那么无脑。

## 核心要点

- **本质**：由 Anthropic 发布的**开放标准**（开源协议），不是某个产品的私有功能。设计目标是成为「AI 界的 HTTP 协议」。

- **解决什么问题**：
  - 以前：每接一个新工具（查数据库、读 Slack、操作浏览器）都要写一遍专属适配代码
  - 现在：工具提供方实现一次 MCP 服务器，所有支持 MCP 的 AI（Claude Code、Cursor 等）都能直接调用

- **技术架构**：
  - **MCP 服务器**：中间人程序，一边对接外部系统（PostgreSQL、GitHub、Sentry），一边用 MCP 协议与 AI 对话
  - **传输方式**：HTTP（远程云服务）、stdio（本地脚本/CLI）、SSE（旧版，已逐步弃用）
  - **数据格式**：标准化的工具定义（JSON Schema）、资源引用（`@server:path`）、提示模板

- **安全机制**：
  - **OAuth 2.0**：用户显式授权（像登录第三方 App），令牌存在系统钥匙串
  - **范围控制**：本地/项目/用户三级配置，敏感操作可设「需人工确认」
  - **Prompt 注入防护**：不信任的 MCP 服务器内容会被标记，防止恶意数据污染 AI 决策

- **Cursor vs Claude Code 的支持**：

| 产品 | MCP 支持程度 | 配置方式 | 特点 |
|------|-------------|----------|------|
| **Claude Code** | 原生深度集成 | `claude mcp add` 命令 | 200+ 官方/社区服务器；`@` 引用资源；`--scope` 三级配置 |
| **Cursor** | 间接支持（插件/Commands） | 插件市场或 Custom Commands | 生态较新，主要靠社区贡献 |

## 应用场景

- **开发工作流自动化**：
  - 「根据 JIRA ticket ENG-4521 的描述写代码，并自动创建 GitHub PR」
  - 「查询 Sentry 的错误日志，定位问题代码，运行测试验证修复」
- **数据驱动的决策**：
  - 「查 PostgreSQL 数据库，找出过去 30 天未登录的用户，发 Gmail 召回邮件」
  - 「读取 Figma 设计稿，按设计规范更新组件代码」
- **跨系统协作**：
  - 让 AI 同时操作 GitHub、Slack、Notion，实现「PR 合并后自动发 Slack 通知并更新文档」
- **本地工具链整合**：
  - 把内部 CLI 工具包装成 MCP 服务器，让 AI 能调用私有部署的测试平台或构建系统

## 相关概念

- **Tool Use（工具使用）**：MCP 是 Tool Use 的工程化、标准化实现；Tool Use 是能力，MCP 是协议
- **RAG（检索增强生成）**：MCP 可动态拉取实时数据，弥补 RAG 静态索引的滞后性；两者常配合使用
- **Agent（代理）**：MCP 是 Agent 的「手臂」，让 Agent 能伸到外部世界拿信息、执行动作
- [[AI概念--Skill]]（MCP 是「外部接口」，Skill 是「内部工作流」——MCP 让 AI 能查数据库，Skill 教 AI「查到数据后怎么处理」）
- [[AI概念--上下文]]（MCP 拉取的数据会进入 AI 上下文窗口，占用 token）
- [[AI概念--Token]]（MCP 返回的数据量直接消耗 token 配额）
- [[AI概念--幻觉]]（MCP 实时查询可减少知识过时导致的幻觉）
- [[AI概念--Agent]]（MCP 是 Agent 的「手臂」，Agent 通过 MCP 连接外部世界）
- [[AI 代理核心概念速查]]（MCP 在 AI 能力扩展层的位置，与 Skills 并列）

## 来源

- [Claude Code 文档 - 通过 MCP 将 Claude Code 连接到工具](https://code.claude.com/docs/zh-CN/mcp)
- [Model Context Protocol 官方规范](https://modelcontextprotocol.io/introduction)
- [Anthropic MCP Registry - 官方服务器列表](https://api.anthropic.com/mcp-registry/docs)
