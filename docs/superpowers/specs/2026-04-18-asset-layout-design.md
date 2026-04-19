# 资源目录规范调整设计

**目标：**统一流程图与附件的存放位置，减少 `raw/assets/` 根目录混杂文件，明确后续 ingest 与知识库维护的路径约定。

## 约束

- 流程图文件统一放到 `x/` 目录，覆盖 `.canvas`、`.excalidraw`、`.excalidraw.md`。
- `raw/articles/foo.md` 的附件归档到 `raw/assets/articles/foo/`。
- 仅出现在 `wiki/concepts/...` 的图片归档到 `raw/assets/wiki/concepts/<topic>/<页面名>/`。
- `wiki/` 不建立独立附件目录，继续引用 `raw/assets/` 与 `x/` 中的文件。
- `raw/` 的来源正文不可被 AI 改写内容，但允许为本次结构治理更新附件相对路径引用。
- `wiki/log.md` 只追加，不修改历史记录。

## 现状

- 仓库中已有 `x/mcp_excalidraw_flow.excalidraw`，但仍有一个流程图放在 `raw/assets/vpn-tunnel-flow.excalidraw`。
- 多个 `raw/articles/*.md` 仍使用 `../assets/<文件名>` 这种扁平路径。
- 多个 `wiki/*.md` 仍直接引用 `raw/assets/<文件名>`；其中只有 `vpn-tunnel-flow.excalidraw` 可以确认需要实际迁移。
- 当前工作区可见的真实附件文件极少，因此本次迁移分为“物理迁移真实存在文件”与“更新路径规则/引用”两部分。
- 历史图片中存在大量“有引用、无文件”的情况；这类坏链本次不做猜测性重写，保持原状并在汇报中说明。

## 设计决策

### 1. 流程图目录

- 所有流程图资产集中在仓库根目录 `x/`。
- `wiki/` 或 `raw/` 中引用流程图时，直接引用 `x/` 中的目标文件。
- 这样便于人工统一管理，也避免图文件混在原始内容附件目录中。

### 2. raw 附件目录

- 来源文件 `raw/articles/foo.md` 的附件归档到 `raw/assets/articles/foo/`
- 来源文件 `raw/videos/bar.md` 的附件归档到 `raw/assets/videos/bar/`
- 路径规则为：

```text
raw/<kind>/<name>.md
-> raw/assets/<kind>/<name>/
```

```text
wiki/concepts/<topic>/<page>.md 中引用的图片
-> raw/assets/wiki/concepts/<topic>/<page>/
```

### 3. wiki 侧处理

- 不为 `wiki/` 定义新的附件写入规则。
- `wiki` 只消费 `raw/assets/...` 与 `x/...` 中的资源。
- 能确定目标位置的旧引用直接改到新路径；无法安全推断来源的旧引用暂不机械改写，避免把路径改错。

### 4. 文档与指令

- 在 `CLAUDE.md` 中更新目录结构与图片/流程图规则。
- 在 `.claude/commands/ingest.md` 中补充附件与流程图的路径约束。
- 额外增加 `.claude/rules/asset-layout.mdc`，把上述规则固化为 Claude Code 会话级约束。

## 实施范围

### 必做

- 更新规范文档
- 创建 Cursor 规则文件
- 将 `raw/assets/vpn-tunnel-flow.excalidraw` 迁移到 `x/vpn-tunnel-flow.excalidraw`
- 更新已知受影响引用
- 在 `wiki/log.md` 追加一次结构治理记录

### 有意不做

- 不批量改写所有 `wiki/` 图片路径到未知目标目录
- 不凭猜测创建缺失的历史附件文件
- 不修改 `wiki/log.md` 历史条目

## 验证

- 检查是否还有 `.canvas`、`.excalidraw`、`.excalidraw.md` 文件留在 `x/` 之外
- 检查 `CLAUDE.md` / `ingest` 命令 / Cursor 规则是否三处一致
- 检查 `vpn-tunnel-flow.excalidraw` 的旧路径引用是否全部更新
- 检查没有把未知归属的历史坏链做猜测性改写
