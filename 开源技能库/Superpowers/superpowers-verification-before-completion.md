# verification-before-completion (Superpowers)

Superpowers 的完成前验证 skill。**在说"完成了"之前，先证明它确实完成了。**

## 核心原则

不接受"应该没问题了"或"逻辑上应该能工作"。必须：

1. 运行相关测试，确认全部通过
2. 检查**具体的问题点**（不是泛泛地"测试通过"）
3. 验证没有引入新的失败

## 为什么重要

LLM 的自然倾向是在完成修改后直接声称"已修复"，而不实际验证。这个 skill 强制打破这个模式。

与 弱成功标准 缺陷直接对应：Karpathy 指出 LLM 在没有明确成功标准时无法自主验证。`verification-before-completion` 把验证步骤内置进完成流程。

## 在 Superpowers 流程中的位置

每个任务完成时触发，不只是最终交付时：

```
executing-plans → [verification-before-completion] → 下一个任务

```


## 相关

- entities/superpowers — Superpowers 完整介绍
- skills/superpowers-systematic-debugging — 调试时的配套 skill
- concepts/llm-coding-pitfalls — 弱成功标准缺陷
- skills/superpowers-requesting-code-review — 验证通过后的代码审查
