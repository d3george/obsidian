---
created: 2026-04-28
updated: 2026-04-28
title: "Git worktrees aren't the problem"
author: "The Modern Coder"
url: "https://www.youtube.com/watch?v=A2f3T1JELbo"
raw: "[[📺-Git worktrees aren't the problem]]"
---

# Git worktrees aren't the problem

## 摘要

worktree 概念简单，但三个容易被忽略的细节会让体验从"顺手"变成"别扭"：创建时需明确分支指针、磁盘上放在哪里、被忽略文件为何会缺失。视频从实战角度逐一拆解这些细节，帮助开发者从"会用命令"进阶到"理解取舍"。

## 核心要点

- **创建三要素**：`git worktree add` 需要名称（路径）、起点、分支三者明确；当目标分支已被其他 worktree 占用时会报错，用 `-b` 创建新分支是最佳实践
- **磁盘布局**：worktree 放在项目目录**同级**比放在内部更干净——避免 IDE 文件树混乱和 gitignore 管理负担；Claude Code 将其放在 `.claude/worktrees/` 下并 gitignore
- **被忽略文件不会复制**：worktree 仅复制 tracked 文件，`node_modules` 等构建产物需重建、`.env` 等 secrets 需手动拷贝；这是正确行为，不是 bug
- **取舍认知**：worktree = 用初始化摩擦换干净的上下文切换；如果这个取舍不适合，`git stash` 才是正确工具

## 提取的概念

- [[git-worktree]] — 补充创建三要素、磁盘布局、忽略文件处理等细节
