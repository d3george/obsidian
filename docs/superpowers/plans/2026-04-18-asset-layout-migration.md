# Asset Layout Migration Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将流程图与 raw 附件目录规范写入仓库文档，并完成当前可确认资源与引用的迁移。

**Architecture:** 先修改规则文档，再迁移真实存在的流程图文件，随后更新 markdown 引用并追加日志。对无法安全推断目标目录的历史图片引用保持克制，只改确定项，避免制造新的坏链。

**Tech Stack:** Markdown、Cursor rules、仓库内文档约束、简单文本迁移

---

### Task 1: 固化目录规范

**Files:**
- Create: `docs/superpowers/specs/2026-04-18-asset-layout-design.md`
- Create: `docs/superpowers/plans/2026-04-18-asset-layout-migration.md`
- Create: `.claude/rules/asset-layout.mdc`
- Modify: `CLAUDE.md`
- Modify: `.claude/commands/ingest.md`

- [ ] **Step 1: 写入规则文件**

创建 `.claude/rules/asset-layout.mdc`，声明：
- 流程图放 `x/`
- `raw/<kind>/<name>.md` 的附件放 `raw/assets/<kind>/<name>/`
- `wiki/concepts/<topic>/<page>.md` 的图片放 `raw/assets/wiki/concepts/<topic>/<page>/`
- `wiki/` 只引用 `raw/assets` 与 `x/`

- [ ] **Step 2: 更新仓库主规范**

修改 `CLAUDE.md` 的目录结构、图片规范、流程图规范，使其与设计文档一致。

- [ ] **Step 3: 更新 ingest 指令**

修改 `.claude/commands/ingest.md`，加入：
- 图片引用仍用标准 markdown
- `raw/articles/foo.md` 的附件落在 `raw/assets/articles/foo/`
- `wiki/concepts/...` 的图片落在 `raw/assets/wiki/concepts/.../`
- 流程图落在 `x/`

- [ ] **Step 4: 手工检查三处规则是否一致**

核对 `CLAUDE.md`、`.claude/commands/ingest.md`、`.claude/rules/asset-layout.mdc` 的说法没有冲突。

### Task 2: 迁移已确认的流程图

**Files:**
- Create: `x/vpn-tunnel-flow.excalidraw`
- Modify: `wiki/sources/macOS VPN 隧道内网访问方案.md`
- Modify: `wiki/log.md`
- Delete: `raw/assets/vpn-tunnel-flow.excalidraw`

- [ ] **Step 1: 复制流程图内容到新位置**

把 `raw/assets/vpn-tunnel-flow.excalidraw` 的内容原样写入 `x/vpn-tunnel-flow.excalidraw`。

- [ ] **Step 2: 更新已知引用**

修改 `wiki/sources/macOS VPN 隧道内网访问方案.md` 中的引用，并在新的日志条目中记录新路径 `x/vpn-tunnel-flow.excalidraw`；不要回写历史日志。

- [ ] **Step 3: 删除旧文件**

确认新文件已创建后删除 `raw/assets/vpn-tunnel-flow.excalidraw`。

### Task 3: 处理可确认的 raw 附件引用

**Files:**
- Inspect only: `raw/articles/开发者必备神兵：tmux 简明介绍.md`
- Inspect only: `raw/articles/Obsidian 云端同步最佳方案.md`
- Inspect only: `raw/articles/obsidian 附件管理最佳实践.md`

- [ ] **Step 1: 只处理“文件真实存在且归属明确”的引用**

示例目标规则：
- `raw/articles/开发者必备神兵：tmux 简明介绍.md`
- `-> raw/assets/articles/开发者必备神兵：tmux 简明介绍/`

如果目标附件文件不存在，或无法确认归属，则保持原引用不动。

- [ ] **Step 2: 明确记录跳过原因**

把“历史坏链未重写”的原因写入最终汇报和日志描述，不制造新的猜测路径。

### Task 4: 记录与验证

**Files:**
- Modify: `wiki/log.md`

- [ ] **Step 1: 追加结构治理日志**

在 `wiki/log.md` 追加一次 `maint` 或 `lint-fix` 风格的记录，说明：
- 新增规则
- 流程图迁移
- 已更新的引用文件

- [ ] **Step 2: 验证残留**

检查：
- 是否还有 `.canvas`、`.excalidraw`、`.excalidraw.md` 文件留在 `x/` 之外
- 是否还有对 `raw/assets/vpn-tunnel-flow.excalidraw` 的引用
- 规则文档是否一致

- [ ] **Step 3: 汇报限制项**

说明当前仓库中许多历史图片仅有引用、无对应文件，因此本次未做不安全的批量物理迁移。
