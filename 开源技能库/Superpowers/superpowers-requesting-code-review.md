# requesting-code-review (Superpowers)

Superpowers 的代码审查发起 skill。每个子任务完成后**自动触发**，不是可选步骤。

## 严重程度分级

| 级别 | 含义 | 对流程的影响 |
| ------------ | -------------------------------- | ------------------------------ |
| **Critical** | 安全漏洞、数据丢失风险、逻辑错误 | **阻断流程**，必须修完才能继续 |
| **High** | 性能问题、可维护性严重下降 | 强烈建议修复 |
| **Medium** | 代码风格、小的设计问题 | 建议修复 |
| **Low** | 细节优化、可选改进 | 可选 |

## 关键机制

**Critical 问题阻断流程**：发现 Critical 问题时，不能继续下一个任务，必须先解决。这是 Superpowers 保证代码质量的核心机制之一。

## 与 receiving-code-review 的关系

`requesting-code-review` 是**发起**审查并上报问题；receiving-code-review 是**处理**审查反馈，用技术判断力决定接受还是拒绝建议。两个 skill 配对使用。

## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-receiving-code-review — 配对 skill：处理审查反馈
- skills/superpowers-executing-plans — 上一步：执行计划
- skills/superpowers-verification-before-completion — 审查前的验证
