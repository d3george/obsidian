# Wiki Log

> Append-only 操作日志。格式：`## [YYYY-MM-DD] 操作类型 | 描述`

## [2026-04-12] lint | 全库检查，发现 30 个问题，已修复：6 个 AI 概念页死链接（AI概念--上下文→Context、MCP→AI概念--MCP）、4 个视频死链接转为纯文本引用、Brand Guidelines Prompt 拼写修复（sou→source）、6 个页面补充 # 一级标题、22 个概念页补充 frontmatter（created/updated/sources）、创建 wiki/sources/ 目录

## [2026-04-12] ingest | tmux 简明介绍、How to Build a MVP、掌握Docker的核心概念
- 创建：wiki/sources/tmux 简明介绍.md、wiki/sources/How to Build a Minimum Viable Product (MVP).md、wiki/sources/掌握Docker的核心概念.md
- 更新：wiki/concepts/devops/tmux-cheatsheet.md（+1 source）、wiki/concepts/product/MVP.md（+1 source）、wiki/concepts/devops/掌握Docker的核心概念而非死记命令.md（+1 source）
- 更新：wiki/index.md（来源摘要表、统计数据）

## [2026-04-16] ingest | macOS VPN 隧道内网访问方案 + Excalidraw 流量流转图
- 创建：wiki/sources/macOS VPN 隧道内网访问方案.md（关联 raw/assets/vpn-tunnel-flow.excalidraw）
- 创建 6 个概念页：SSH 隧道、Clash Verge Rev、Fake-IP DNS、FortiClient VPN、Parallels 端口转发、SOCKS5 代理
- 更新：wiki/index.md（+Networking 分类、+1 来源摘要、概念笔记 21→27、raw 统计 10→9）

## [2026-04-12] lint | 全库检查，发现 8 个问题：5 个 system-design 页面引用已删除的 TV 文件（死链接）、5 个孤立页面（仅 index.md 引用）、raw/Articles 中 Portable REST Query 文件已删除但 index 统计未更新

## [2026-04-12] lint-fix | 修复 8 个问题
- 移除 5 个 system-design 页面中指向已删除 TV 文件的死链接，GraphQL/gRPC 补充 SOAP 交叉引用
- index.md 移除 TV 条目，概念笔记 22→21，raw 统计 11→10
- 为 5 个孤立页面添加交叉引用：SOLID→REST API/gRPC、Brand Guidelines Prompt→Skill/幻觉、superpowers→Skill/Agent、How Git Works→tmux/Docker、zed-vim-cheatsheet→tmux/superpowers

## [2026-04-18] maint | 资源目录规范收口
- 新增规则：流程图统一放在 `x/`；`raw/` 来源附件统一放在 `raw/assets/<来源相对路径去后缀>/`
- 新增：`.cursor/rules/asset-layout.mdc`、`docs/superpowers/specs/2026-04-18-asset-layout-design.md`、`docs/superpowers/plans/2026-04-18-asset-layout-migration.md`
- 更新：`CLAUDE.md`、`.claude/commands/ingest.md`、`wiki/sources/macOS VPN 隧道内网访问方案.md`
- 迁移：`raw/assets/vpn-tunnel-flow.excalidraw` → `x/vpn-tunnel-flow.excalidraw`
- 说明：历史图片中存在大量“有引用、无文件”的坏链，本次未做猜测性批量重写

## [2026-04-19] maint | raw/assets 附件分桶迁移
- 将 `raw/assets/` 根目录下的 60 个附件文件迁移到分桶目录，并同步更新 `raw/**/*.md` 与 `wiki/**/*.md` 中的引用路径
- `raw/articles/*.md` 的附件落在 `raw/assets/articles/<来源文件名去后缀>/`
- 仅出现在 `wiki/concepts/...` 的图片落在 `raw/assets/wiki/concepts/<topic>/<页面名>/`
- 无引用附件落在 `raw/assets/_orphan/`（3 个 png）
- 修正：`raw/articles/vpn-tunnel-flow.md` 中 gif 引用路径

## [2026-04-19] maint | 清理 orphan 附件并收敛规则到 .claude
- 删除：`raw/assets/_orphan/image-2025-12-21-11-35-37.png`、`raw/assets/_orphan/image-2026-03-19-10-38-55.png`、`raw/assets/_orphan/image-2026-03-19-10-40-21.png`
- 规则迁移：`.cursor/rules/asset-layout.mdc` → `.claude/rules/asset-layout.mdc`，并移除 `.cursor/` 目录
- 更新：`docs/superpowers/specs/2026-04-18-asset-layout-design.md`、`docs/superpowers/plans/2026-04-18-asset-layout-migration.md`

## [2026-04-19] lint | 全库检查
- 扫描页面数：31（不含 `wiki/index.md`、`wiki/log.md`）
- 发现问题数：19（孤立 0、歧义链接 0、死链接 1、frontmatter 缺失 0、过期 0、`[!contradiction]` 0；另有 18 篇概念页 `sources: 0` 作为“可追溯性/知识缺口”信号）
- 关键发现：`wiki/sources/macOS VPN 隧道内网访问方案.md` 曾用 wikilink 指向 `x/vpn-tunnel-flow.excalidraw`，无法按笔记 stem 解析为 `.md`；其余 wikilink 在当前命名规则下未出现重复 stem 歧义
- 报告：见对话输出《Wiki Lint 报告》（未自动修复）

## [2026-04-19] fix | lint 死链与来源 raw 字段
- `wiki/sources/macOS VPN 隧道内网访问方案.md`：关联图表改为 Markdown 相对路径 `../../x/vpn-tunnel-flow.excalidraw`，并更新 `updated`
- `wiki/sources/掌握Docker的核心概念.md`：`raw` 改为明示「无独立 raw 文稿、仅视频 url」，避免与 `raw/` 目录语义混淆；更新 `updated`
- `wiki/log.md`：调整同日 lint 条目中示例表述，避免行内出现 wikilink 双方括号语法被图谱/脚本误判

