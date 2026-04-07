---
tags:
  - type/concept
  - topic/ai
  - source/article
  - status/done
---

# AI Skill

## 定义

**一句话：** Skill 是打包好的「工作流说明书」——让 AI 助手在特定场景下自动切换成专家模式，按预设的步骤、检查清单和边界条件执行任务，而不需要你每次重复解释。

**比喻：** 像**餐厅后厨的「标准操作手册（SOP）」**：新手厨师翻到哪道菜的手册，就按步骤切配、火候、装盘，保证出品一致；AI 读到哪个 Skill，就按里面的规则、流程、禁用事项来响应，不跑偏。

> **对应关系**：「手册」对应 **Skill 的 Markdown 文档**（SKILL.md）；「哪道菜」对应 **触发条件**（description 里的症状/场景）；「步骤与禁忌」对应 **Core Pattern、When NOT to use、Checklist**；「后厨自动按手册做」对应 **Agent 检测到场景匹配后加载 Skill 并遵循其约束**。

## 核心要点

- **来源与定位**：Anthropic（Claude 的母公司）在 2025 年 10 月推出，作为 **Claude Code 的能力扩展机制**；与 Cursor 的 Custom Commands 类似，但更强调「条件触发 + 完整工作流」而非单一提示词。

- **本质不是代码库，是「流程模板」**：Skill 本身是纯文本（Markdown + YAML frontmatter），定义何时用、怎么用、何时别用、常见错误；可以附带脚本，但核心是「说明书」。

- **Claude Code 中的 Skill 体系**：
  - **位置**：`~/.claude/skills/` 或项目内 `.claude/skills/`
  - **结构**：`skill-name/SKILL.md` + 可选的支持文件
  - **触发**：通过 YAML frontmatter 的 `description` 描述症状，Agent 判断匹配后加载
  - **类型**：
    - **Technique**：具体方法步骤（如 TDD、条件等待）
    - **Pattern**：思维方式（如复杂度分解）
    - **Reference**：API/工具文档速查

- **Cursor 中的对应概念**：
  - **Skills/Custom Commands**：Cursor 官方文档中提到的模块化专业工作流
  - **.cursor/rules/ `*.mdc` 文件**：类似 Skill 的「持久化指令」，放在项目里随代码库走，定义编码标准、架构决策

- **关键机制对比**：

| 产品 | 技能/规则载体 | 触发方式 | 特点 |
|------|-------------|----------|------|
| **Claude Code** | `SKILL.md` + `.claude/skills/` | `description` 描述症状匹配 | 跨项目复用、Agent 自主加载 |
| **Cursor** | `.cursor/rules/*.mdc` + Custom Commands | 文件匹配（glob）、命令面板调用 | 与项目绑定、IDE 原生集成 |

- **为何用 Skill 而非直接说**：
  - **省 Token**：复杂流程写一次，以后只加载 Skill 文件名，不必每次带长篇 prompt
  - **保一致性**：多人协作、多会话间，AI 行为不因对话历史漂移
  - **可验证**：Skill 可测试（TDD 思路：先跑失败场景，确认 Skill 补足了盲区）

## 应用场景

- **封装重复工作流**：「做 Code Review」「写测试」「部署检查」——写成 `/review` Skill，以后一句指令启动完整流程。
- **固化团队规范**：把「怎么写 React 组件」「错误处理标准」写成 Skill，新成员也能得到一致辅导。
- **跨项目复用**：通用的「系统调试技巧」「性能优化模式」放在 `~/.claude/skills/`，任何项目都能唤出。

## 相关概念

- **MCP（Model Context Protocol）**：Skill 是「内部工作流」，MCP 是「外部工具接口」——Skill 教 AI 怎么思考，MCP 让 AI 能查数据库、调 API。
- [[AI概念--MCP]]（MCP 与 Skill 分工：外部接口 vs 内部流程）
- **Prompt Engineering**：Skill 是「高级 Prompt」的工程化、版本化、可复用化。
- **Agent 模式**：Skill 在 Agent 模式下被加载，决定 Agent 的行为边界和工具使用顺序。
- **Cursor Rules**：Cursor 中与 Skill 最接近的对应物，`.mdc` 文件定义项目级持久规则。
- [[AI概念--上下文]]（Skill 按需加载，节省上下文空间）
- [[AI概念--Token]]（Skill 按需加载的核心目的是节省 token 消耗）
- [[AI概念--幻觉]]（Skill 可约束 AI 减少偏离和编造）
- [[AI概念--Agent]]（Skill 是 Agent 的「工作手册」，Agent 调用 Skill 规范行为）
- [[AI 代理核心概念速查]]（Skills 与 MCP、Prompt 的关系）

## 来源

- [Claude Code 文档 - Skills 介绍](https://code.claude.com/docs/zh-CN/skills)
- [Cursor 文档 - Custom Commands 与 Skills](https://cursor.com/docs)（上下文与能力扩展章节）
- [Anthropic 官方博客 - Agent Skills 发布](https://www.anthropic.com/blog/skills)（2025-10）
