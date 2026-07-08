# Test-Driven Development (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

**TDD** 解决「AI 修 bug 为什么要先写测试」的问题。先写测试迫使你明确定义问题，防止 AI 假修，并为后续重构提供安全网。

相关：concepts/vertical-slice-tdd · concepts/feedback-loop-driven-debugging · skills/debugging-and-error-recovery/debugging-and-error-recovery

> 注：本页面来自 古法编程.com 的 TDD 实践视角，与 concepts/vertical-slice-tdd 中 Matt Pocock 的垂直切片 TDD 互补。

## AI 修 Bug 的常见失败模式

- **改了症状没改原因**：表面上不报错了，根本原因还在
- **改了这里坏了那里**：没有测试保护，牵一发动全身
- **假修**：AI 说「修好了」，但没有实际验证过

## TDD 修 Bug 流程

```
红：先写一个能重现 bug 的测试（测试现在应该失败）
↓
绿：让 AI 修改代码直到测试通过
↓
蓝：有了测试保护，安全地重构

```


## 给 AI 的指令模式

> 「先写一个测试来重现这个 bug，测试应该现在失败。等我确认测试正确后再开始修复。」

关键点：

- 分两步：先写测试，确认测试失败后再修复
- 不要让 AI 同时写测试和修复——会导致测试配合实现而测试驱动实现

## 先写测试的三重价值

1. **确认真正理解了 bug**：能写出失败测试，说明你理解了问题
2. **防止 AI 假修**：修复必须让测试从红变绿，不能蒙混过关
3. **修完有回归保护**：这个 bug 从此被永久钉住，不会再次出现

## 与 Vertical Slice TDD 的关系

Vertical Slice TDD 强调**新功能开发**时一条测试对应一条实现的节奏。

本技能（古法编程 TDD）聚焦**修复已有 bug** 时先建立测试再修复的流程。

两者核心一致：测试先于实现，红绿是信号。

## 与 Feedback Loop Driven Debugging 的关系

Feedback-Loop-Driven Debugging 强调先建立快速反馈回路。

TDD 是建立反馈回路的最高效方式——一条失败测试就是最精确的反馈信号。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`test-driven-development`

**本地文件**：`skills/test-driven-development/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/test-driven-development" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/test-driven-development" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/test-driven-development/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/test-driven-development/SKILL.original.md` 替换 `SKILL.md`。
