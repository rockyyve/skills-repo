# receiving-code-review (Superpowers)

Superpowers 的审查反馈处理 skill。**不盲目接受所有审查建议**，用技术判断力过滤。

## 核心原则

AI 审查者可能：

- 提出有效的 Critical 问题（必须修）
- 提出过度设计的建议（可以拒绝）
- 误解上下文（需要解释）

`receiving-code-review` 的作用是让 AI 在接收审查时**保持技术主体性**，而不是把所有建议都当成命令执行。

## 处理流程

1. 按严重程度分类（Critical 优先）
2. 对每条建议评估：是否有充分理由？是否符合项目约束？
3. Critical 问题：修复
4. 其他问题：技术判断后决定接受/拒绝/推迟
5. 拒绝时给出理由

## 与 requesting-code-review 的关系

requesting-code-review 发起审查并上报问题；`receiving-code-review` 处理这些问题。两个 skill 是一对。

## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-requesting-code-review — 配对 skill：发起审查
- concepts/surgical-changes — 相关原则：只改必须改的
