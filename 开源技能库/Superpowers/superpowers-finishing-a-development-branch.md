# finishing-a-development-branch (Superpowers)

Superpowers 的分支收尾 skill。在 worktree 工作完成后，**决定如何处理这个分支**。

## 收尾检查清单

合并前必须确认：

- [ ] 所有测试通过
- [ ] 代码审查 完成，Critical 问题已修复
- [ ] 验证 完成，功能符合预期
- [ ] 提交信息清晰，描述了做了什么和为什么

## 四个选项

| 选项 | 适用场景 |
| --------------- | ------------------------------ |
| **合并到 main** | 检查清单全部通过，可以直接合并 |
| **创建 PR** | 需要团队审查，或遵循 PR 工作流 |
| **保留分支** | 工作未完成，稍后继续 |
| **放弃** | 方向错误，丢弃所有改动 |

## 与 using-git-worktrees 的关系

using-git-worktrees 负责创建和使用 worktree；`finishing-a-development-branch` 负责收尾。两个 skill 是一对。

## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-using-git-worktrees — 配对 skill：创建 worktree
- skills/superpowers-requesting-code-review — 收尾前的代码审查
