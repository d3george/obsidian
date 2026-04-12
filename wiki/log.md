# Wiki Log

> Append-only 操作日志。格式：`## [YYYY-MM-DD] 操作类型 | 描述`

## [2026-04-12] lint | 全库检查，发现 30 个问题，已修复：6 个 AI 概念页死链接（AI概念--上下文→Context、MCP→AI概念--MCP）、4 个视频死链接转为纯文本引用、Brand Guidelines Prompt 拼写修复（sou→source）、6 个页面补充 # 一级标题、22 个概念页补充 frontmatter（created/updated/sources）、创建 wiki/sources/ 目录

## [2026-04-12] ingest | tmux 简明介绍、How to Build a MVP、掌握Docker的核心概念
- 创建：wiki/sources/tmux 简明介绍.md、wiki/sources/How to Build a Minimum Viable Product (MVP).md、wiki/sources/掌握Docker的核心概念.md
- 更新：wiki/concepts/devops/tmux-cheatsheet.md（+1 source）、wiki/concepts/product/MVP.md（+1 source）、wiki/concepts/devops/掌握Docker的核心概念而非死记命令.md（+1 source）
- 更新：wiki/index.md（来源摘要表、统计数据）

## [2026-04-12] lint | 全库检查，发现 8 个问题：5 个 system-design 页面引用已删除的 TV 文件（死链接）、5 个孤立页面（仅 index.md 引用）、raw/Articles 中 Portable REST Query 文件已删除但 index 统计未更新

## [2026-04-12] lint-fix | 修复 8 个问题
- 移除 5 个 system-design 页面中指向已删除 TV 文件的死链接，GraphQL/gRPC 补充 SOAP 交叉引用
- index.md 移除 TV 条目，概念笔记 22→21，raw 统计 11→10
- 为 5 个孤立页面添加交叉引用：SOLID→REST API/gRPC、Brand Guidelines Prompt→Skill/幻觉、superpowers→Skill/Agent、How Git Works→tmux/Docker、zed-vim-cheatsheet→tmux/superpowers

