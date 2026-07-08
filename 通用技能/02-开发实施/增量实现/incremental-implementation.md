# Incremental Implementation

> **Skill 文件**：SKILL.md · SKILL.original.md

**Incremental Implementation** 解决「AI 写代码怎么小步推进」的问题。每次只实现一个可验证的小步骤，而不是一次性实现整个功能。

相关：skills/api-and-interface-design/api-and-interface-design · concepts/vertical-slice-tdd · concepts/feedback-loop-driven-debugging

## 核心原则

> 一次只向前走一步，走完确认，再走下一步。

小步的三个标准：

1. **能独立运行**：不依赖尚未实现的其他部分
2. **能独立测试**：有明确的验证方式
3. **失败时能定位**：出错时知道问题在哪一步

## 操作方式

```
把任务拆成步骤序列
→ 每步完成后验证
→ 验证通过再进下一步
→ 失败时影响范围仅限当前步骤

```


给 AI 的指令模式：

> 「只实现这一步：[具体步骤]。完成后停下来等我确认，不要继续下一步。」

## 防止 AI 跳步

AI 的默认行为是「一口气写完」。必须显式约束：

- 明确告知「只实现这一步」
- 每步完成后要求 AI 解释做了什么
- 验证通过后才给出下一步指令

如果发现 AI 跑偏（实现了不该实现的部分），**立即正**，不要等到最后再处理。

## 失败处理

小步失败的优势：

| 一次性实现 | 小步实现 |
|-----------|----------|
| 失败时不知道哪里出错 | 失败范围只有当前步骤 |
| 回滚成本高 | 回滚只影响最近一步 |
| 调试时需要全局搜索 | 调试范围已被隔离 |

## 与相关概念的关系

- **task-breakdown**（任务级拆分）：把一个功能拆成多个任务
- **incremental-implementation**（实现级拆分）：把一个任务拆成多个可验证步骤
- **Vertical Slice TDD**：TDD 场景下的增量实现——一条测试对应一步实现

两层拆分结合使用：先 task-breakdown 确定做什么，再 incremental-implementation 确定怎么做。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`incremental-implementation`

**本地文件**：`skills/incremental-implementation/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/incremental-implementation" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/incremental-implementation" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/incremental-implementation/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/incremental-implementation/SKILL.original.md` 替换 `SKILL.md`。
