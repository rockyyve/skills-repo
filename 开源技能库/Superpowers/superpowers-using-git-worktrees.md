# using-git-worktrees (Superpowers)

Superpowers 的 Git Worktree 隔离 skill。**每个任务在独立分支上工作，不污染 main。**

## 核心流程

1. 接手任务时**自动创建独立 worktree**
2. 在新分支上工作
3. 开始前验证测试基线干净（所有测试通过）
4. 完成后展示选项：
 - 合并到 main
 - 创建 PR
 - 保留分支（稍后处理）
 - 放弃（丢弃所有改动）

## 为什么重要

没有 worktree 隔离时，AI 可能在 main 分支上直接修改，出错时难以回滚。Worktree 隔离把每个任务变成可独立撤销的单元。

**测试基线验证**：开始工作前确认测试全绿，这样任务完成后的测试失败一定是本次改动引入的，不是历史遗留问题。

## 与 finishing-a-development-branch 的关系

`using-git-worktrees` 负责**创建和使用** worktree；finishing-a-development-branch 负责**收尾决策**（合并策略、PR 描述等）。

## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-finishing-a-development-branch — 配对 skill：分支收尾
- concepts/multi-agent-orchestration — worktree 是多 Agent 并行的基础设施
