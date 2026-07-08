# triage

mattpocock/skills 中的 issue 分诊 skill。管理 issue 输入流，决定哪些可以交给 agent 独立完成。

## 标签体系

两个维度：

| 维度          | 示例值                                                       |
| ------------- | ------------------------------------------------------------ |
| Category role | `bug` / `enhancement` / `chore`                              |
| State role    | `needs-info` / `ready-for-agent` / `in-progress` / `wontfix` |

## 核心输出

每个 issue 经过 triage 后产出一份 **durable agent brief**——一段可以直接粘贴给 agent 的上下文摘要，包含：问题描述、复现步骤（bug）或验收标准（feature）、相关文件路径。

## 设计意图

`triage` 是 issue 的**入口过滤器**。它的目标是让 `ready-for-agent` 的 issue 真的可以被 agent 独立完成，而不是让 agent 在执行中途发现信息不足。

吸收了废弃 skill `qa` 的职责（交互式 QA 报 bug → 创建 issue）。

## 相关

- skills/to-issues/SKILL — 上游：生成 issue 的 skill
- entities/mattpocock-skills — 仓库全貌
