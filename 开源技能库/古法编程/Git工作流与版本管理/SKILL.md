# Git Workflow and Versioning（Git 工作流和版本控制）

## 概述

Git 是你的安全网。把提交当存档点，分支当沙盒，历史当文档。在 AI Agent 高速生成代码时，有纪律的版本控制是让变更可控、可审查和可回滚的机制。

## 什么时候用

始终。每个代码变更都流经 git。

## 核心原则

### Trunk-Based Development（推荐）

保持 `main` 始终可部署。在短生命周期功能分支中工作，在 1-3 天内合并回去。

```
main ──●──●──●──●──●──●──●──●──●── (始终可部署)
 ╲ ╱ ╲ ╱
 ●──●─╱ ●──╱ ← 短生命周期功能分支 (1-3 天)

```


- **开发分支是成本。** 分支每活一天就累积合并风险。
- **发布分支可接受。** 当你需要稳定发布同时 main 继续前进。
- **Feature flags > 长分支。** 优先把未完成的工作放在 flag 后面部署而不是在分支上放几周。

### 1. 尽早提交，频繁提交

每个成功的增量都有自己的提交。不要累积大量未提交变更。

### 2. 原子提交

每个提交做一件事：

```
# 好：每个提交自包含
a1b2c3d Add task creation endpoint with validation
d4e5f6g Add task creation form component
h7i8j9k Connect form to API and add loading state

# 坏：全混在一起
x1y2z3a Add task feature, fix sidebar, update deps, refactor utils

```


### 3. 描述性消息

提交消息解释 *why*，不只是 *what*：

```
# 好：解释意图
feat: add email validation to registration endpoint

Prevents invalid email formats from reaching the database.
Uses Zod schema validation at the route handler level.

# 坏：描述 diff 中显而易见的
update auth.ts

```


**格式：**

```
<type>: <short description>

<optional body explaining why, not what>

```


**Types：**

- `feat` — 新功能
- `fix` — Bug 修复
- `refactor` — 既不修 bug 也不加功能的代码变更
- `test` — 添加或更新测试
- `docs` — 仅文档
- `chore` — 工具、依赖、配置

### 4. 分离关注点

不要把格式变更和行为变更混在一起。不要把重构和功能混在一起。每种变更类型应是独立提交。

### 5. 控制变更大小

目标每提交/PR 约 100 行。超过约 1000 行的变更应拆分。

## 分支策略

### 功能分支

```
main (始终可部署)
 │
 ├── feature/task-creation ← 每个功能一个分支
 ├── feature/user-settings ← 并行工作
 └── fix/duplicate-tasks ← Bug 修复

```


- 从 `main` 分支
- 保持分支短生命周期（1-3 天内合并）
- 合并后删除分支
- 对未完成功能优先用 feature flags 而非长生命周期分支

### 使用 Worktree

用于并行 AI Agent 工作：

```bash
git worktree add ../project-feature-a feature/task-creation
git worktree add ../project-feature-b feature/user-settings

# 每个 worktree 是独立目录，有自己的分支
# Agent 可以并行工作，互不干扰

```


## 存档点模式

```
Agent 开始工作
 │
 ├── 做一个变更
 │ ├── 测试通过？ → 提交 → 继续
 │ └── 测试失败？ → 回滚到上次提交 → 调查
 │
 └── 功能完成 → 所有提交形成干净历史

```


## Pre-Commit 卫生

每次提交前：

```bash
git diff --staged
git diff --staged | grep -i "password\|secret\|api_key\|token"
npm test
npm run lint
npx tsc --noEmit

```


## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "功能做完了再提交" | 一个巨型提交无法审查、调试或回滚。每个切片都提交。 |
| "消息无所谓" | 消息是文档。未来的你和未来的 Agent 需要理解改了什么和为什么。 |
| "以后再 squash" | Squash 破坏了开发叙事。优先从一开始就干净增量提交。 |
| "分支加开销" | 短生命周期分支免费并防止冲突。 |

## 危险信号

- 大量未提交变更累积
- 提交消息如 "fix"、"update"、"misc"
- 格式变更混着行为变更
- 项目没有 `.gitignore`
- 提交 `node_modules/`、`.env` 或构建产物
- 长生命周期分支与 main 大幅分歧
- 对共享分支 force push

## 验证

每个提交：

- [ ] 提交做了一件事
- [ ] 消息解释 why，遵循类型规范
- [ ] 提交前测试通过
- [ ] diff 中没有密钥
- [ ] 没有纯格式变更混着行为变更
- [ ] `.gitignore` 覆盖标准排除
