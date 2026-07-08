# brainstorming (Superpowers)

Superpowers 的需求对齐 skill。触发后 AI 进入苏格拉底式提问模式，**不直接开写代码**。

## 核心行为

1. 提出澄清问题，挖掘隐含需求和约束
2. 把设计方案**分段展示**，每段确认后再继续
3. 生成**设计文档**作为后续实现的基准
4. 只有在需求对齐完成后才进入实现阶段

## 与 mattpocock/skills 的对比

grill-with-docs 是手动触发的反向拷问；`brainstorming` 是 Superpowers 的**强制入口**——每次开始新功能时自动触发，不能跳过（除非明确说"跳过需求确认"）。

两者目标相同：把对齐成本前置，避免事后返工。

## 在 Superpowers 流程中的位置

```
brainstorming → writing-plans → executing-plans → requesting-code-review

```


## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-writing-plans — 下一步：生成细粒度计划
- skills/grill-the-user — mattpocock/skills 的对应 skill
- concepts/ai-coding-pain-points — brainstorming 解决 Misalignment 痛点
