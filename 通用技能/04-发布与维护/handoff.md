# handoff

mattpocock/skills 中的会话交接 skill。把当前会话压缩成交接文档，供另一个 agent 接手继续工作。

## 核心行为

- **不复制**已有 PRD、issue、diff——只引用路径或 URL
- 压缩当前会话的决策上下文、未完成工作、注意事项
- 输出是一份可以直接粘贴给新 agent 的简洁文档

## 设计意图

长会话中 context window 会被历史消耗。`handoff` 是一种**主动 context 管理**手段：在 context 耗尽之前主动压缩，而不是等 agent 开始遗忘。

与 caveman 的区别：`caveman` 压缩的是**表达方式**（去掉废话），`handoff` 压缩的是**会话历史**（提炼决策）。

## 相关

- entities/mattpocock-skills — 仓库全貌
- skills/caveman-mode — 另一种 token 压缩手段
- skills/to-prd/SKILL — handoff 文档通常会引用 PRD
