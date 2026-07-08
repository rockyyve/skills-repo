# Debugging and Error Recovery

> **Skill 文件**：SKILL.md · SKILL.original.md

**Debugging and Error Recovery** 解决「AI 修 bug 老是靠猜」的问题。没有结构化调试流程时，AI 会随机修改多个地方，每次方向不同，永远无法收敛到正确答案。

相关：concepts/feedback-loop-driven-debugging · skills/test-driven-development/test-driven-development · concepts/vertical-slice-tdd

## AI 靠猜的信号

- 连续修改多个不同地方
- 说「可能是 X 导致的」（用可能/也许 = 在猜）
- 每次修改方向不同
- 修了又出新问题，叠加修复而非回滚

## 正确的调试顺序

```
1. 重现 bug（稳定复现步骤）
2. 缩小范围（二分或逐层查）
3. 确认原因（不是假设，是验证过的事实）
4. 最小修改（只改需要改的，不顺手重构）

```


## 给 AI 的约束指令

> 「在确认根本原因前不要修改代码。先告诉我你认为原因是什么，以及怎么验证这个假设。」

这个约束把 AI 的行为从「随机修改」变成「先分析再行动」。

## 错误恢复原则

如果一次修复引入了新问题：

- **立即回滚**，而不是在新问题上叠加修复
- 叠加修复会导致技术债快速积累最终难以厘清
- 回滚后重新从「确认原因」这步开始

## 保留调试证据

把以下内容记录下来（写入文件，不依赖对话记忆）：

- 稳定复现的步骤
- 完整的错误信息
- 每个假设和对应的验证结果
- 最终确认的根本原因

这些证据在下一个对话中继续调试时非常重要——参考 concepts/context-engineering 了解为何文件比对话记忆更可靠。

## 与 Feedback-Loop-Driven Debugging 的关系

Feedback-Loop-Driven Debugging 提供了更结构化的 6 阶段调试方法（Reproduce → Minimise → Hypothesise → Instrument → Fix → Regression-test）。

本技能（古法编程 视角）聚焦于**约束 AI 行为**——防止 AI 随机猜测，强制先分析再修改。两者互补。

## 与 TDD 的关系

先写测试来重现 bug，是「确认原因」的最佳方式。参考 skills/test-driven-development/test-driven-development 了解如何用 TDD 约束 AI 修 bug 的行为。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`debugging-and-error-recovery`

**本地文件**：`skills/debugging-and-error-recovery/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/debugging-and-error-recovery" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/debugging-and-error-recovery" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/debugging-and-error-recovery/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/debugging-and-error-recovery/SKILL.original.md` 替换 `SKILL.md`。
