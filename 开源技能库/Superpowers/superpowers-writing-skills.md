# writing-skills (Superpowers)

Superpowers 的元技能（meta-skill）。**用来创建新的 Superpowers skill**，让系统可以自我扩展。

## 核心作用

Superpowers 的所有 skill 都是规范文件。`writing-skills` 定义了如何写一个好的 skill：

- **触发条件**：什么情况下这个 skill 应该激活
- **行为规范**：AI 应该做什么、不应该做什么
- **验证标准**：怎么判断 skill 执行成功
- **与其他 skill 的关系**：在整体流程中的位置

## 为什么重要

没有元技能，用户只能使用内置 skill。有了 `writing-skills`，用户可以：

- 为特定项目创建定制 skill
- 把团队的工程规范编码成 skill
- 扩展 Superpowers 覆盖新的工程场景

## 与 Superpowers 设计哲学的关系

Superpowers 的 FAQ 明确说明：Skills 完全可自定义。`writing-skills` 是这个可定制性的实现机制——它把"如何约束 AI 行为"这件事本身也变成了一个可学习、可复用的 skill。

## 相关

- entities/superpowers — Superpowers 完整介绍
- entities/mattpocock-skills — 另一个 skill 系统，对比参照
- skills/superpowers-using-superpowers — 系统介绍 skill
