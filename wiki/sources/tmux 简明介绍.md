---
created: 2026-04-12
updated: 2026-04-12
title: 开发者必备神兵：tmux 简明介绍
raw: "[[开发者必备神兵：tmux 简明介绍]]"
---

# tmux 简明介绍

## 摘要
介绍 tmux 的核心价值（会话持久化、灵活布局、高效组织）和三层架构（Session → Window → Pane）。附常用命令速查和 tmux-resurrect 持久化方案。

## 核心要点
- 会话持久化：断开 SSH 后程序继续运行，重连即恢复
- 三层架构：Session（工作空间）→ Window（标签页）→ Pane（分割区域）
- tmux-resurrect + tmux-continuum 实现跨重启的会话保存/恢复

## 提取的概念
- [[tmux-cheatsheet]]（已有页面，已覆盖核心内容）
