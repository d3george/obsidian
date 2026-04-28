---
created: 2026-04-27
updated: 2026-04-28
sources: 1
---

# git worktree

## 定义

`git worktree` 允许在同一仓库上同时 checkout 多个分支到不同目录，每个目录拥有独立的工作区文件，共享同一个 `.git` 对象库和提交历史。

## 核心要点

- **共享仓库历史**：所有 worktree 看到相同的 `git log` 和 refs，不需要重复 fetch
- **分支独占**：一个分支同一时间只能在一个 worktree 中被 checkout
- **独立暂存区**：每个 worktree 有自己的 index 和 HEAD，互不干扰
- **轻量**：只复制工作目录文件，不重复存储 `.git/objects`，比 clone 省磁盘
- **并行开发**：允许同时在不同分支上工作，无需 stash/commit 切换

## 创建三要素

`git worktree add` 需要 Git 明确三件事：

1. **名称（路径）**：决定工作目录名和磁盘位置
2. **起点（commit）**：从哪个提交开始，如 `main`、`HEAD` 或某个 SHA
3. **分支**：在 worktree 内 checkout 哪个分支

```bash
# 看似正确，实则可能报错 —— main 已被主工作树占用
git worktree add hotfix main
# fatal: 'main' is already used by worktree at...

# 正确做法：用 -b 创建新分支
git worktree add path hotfix -b hotfix
```

**简写形式**：`git worktree add hotfix` 会以当前分支为起点，自动创建同名分支并 checkout，适合快速创建。

关键认知：**分支名与 worktree 目录名保持一致**能在目录列表里一眼看出对应关系。

## 磁盘布局策略

worktree 放在哪里直接影响日常体验，但业界**没有公认最佳实践**，关键是有意选择并保持一致性。

### 三种常见布局

| 策略 | 位置 | 优点 | 缺点 |
|------|------|------|------|
| **同级目录** | `../myproject-hotfix/` | IDE 干净、无需 gitignore | 项目根目录旁会堆积目录 |
| **项目内部** | `./myproject/hotfix/` | 目录集中 | IDE 文件树混乱、需 gitignore |
| **专用目录** | `../worktrees/hotfix/` | 集中管理、整洁 | 需统一约定路径 |

### 实践建议

- **首选同级**：`git worktree add ../feature-auth -b feature-auth`，干净隔离
- Claude Code 将 worktree 放在 `.claude/worktrees/` 并自动 gitignore
- 也有人使用 bare repo 模式，完全以 worktree 的集合来组织项目（进阶用法）
- **不要过早优化**：先选一个约定，保持一致性比找到"完美方案"更重要

来源：[[git-worktrees-arent-the-problem]]

## 被忽略文件的行为

worktree 只复制 **tracked 文件**，`.gitignore` 中列出的文件不会带过去。这是**正确行为**，不是 bug。

### 两类会缺失的文件

| 类别 | 示例 | 处理方式 |
|------|------|----------|
| **构建产物** | `node_modules/`、`dist/`、编译输出 | 每个 worktree 独立重建。修改代码后这些文件本就需要重新生成，不存在"复用"价值 |
| **Secrets/本地配置** | `.env`、`API keys`、IDE 配置 | 手动拷贝，每个 worktree 执行一次。**切勿为省事将 secrets 提交到仓库** |

> 进阶提示：可尝试 symlink 构建目录来减少重复安装，但有额外复杂度。

来源：[[git-worktrees-arent-the-problem]]

## 常用命令

```bash
# 创建新 worktree（同时创建分支）-- 同级目录
git worktree add ../feature-auth -b feature-auth

# 简写：以当前分支为起点，自动创建同名分支
git worktree add ../hotfix

# 基于已有分支创建
git worktree add ../bugfix-login bugfix-login

# 列出所有 worktree
git worktree list

# 删除 worktree
git worktree remove ../feature-auth

# 清理已手动删除的 worktree 记录
git worktree prune
```

## 取舍认知：worktree vs stash

worktree 的本质取舍：**用创建时的初始化摩擦，换取干净快速的上下文切换**。

| 场景 | 推荐工具 | 理由 |
|------|----------|------|
| 需要长时间在新分支上工作 | worktree | 一次性 setup 成本，后续切换零摩擦 |
| 短时间切换到其他分支看一眼 | `git stash` | setup 成本高于收益 |
| 中断当前工作处理紧急修复 | worktree | 当前工作原封不动，修复完回来继续 |
| 当前改动很小，马上回来 | `git stash` | 更快，pop 即恢复 |

理解这个取舍，是区分"会用命令"和"真正理解 Git"的分水岭。

来源：[[git-worktrees-arent-the-problem]]

## AI 开发中的应用

在 [[superpowers]] 工作流中，`using-git-worktrees` 作为"执行计划前创建隔离工作区"的标准步骤：

- 多个 AI agent 分配到不同 worktree，并行处理独立任务
- 每个 session 拥有独立的上下文和工作目录，互不污染
- 完成后各自合并回主分支，自动清理

## 相关概念

- [[How Git Works]] — Git 四大区域与代码流转
- [[superpowers]] — Skills 工作流框架，worktree 是其关键节点
- [[git-worktrees-arent-the-problem]] — 来源视频：创建细节、磁盘布局、忽略文件处理

## 来源

- [[git-worktrees-arent-the-problem]] — The Modern Coder，2026
