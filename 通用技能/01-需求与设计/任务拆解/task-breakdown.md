# task-breakdown

> **Skill 文件**：SKILL.md · SKILL.original.md

解决的核心问题：AI 一次改太多文件，导致难以验证和回滚。大任务必须先拆小任务，每个任务能独立验证。

## 任务粒度标准

合格的小任务满足以下全部条件：

- 单次改动 **< 5 个文件**
- 能在 **10-20 分钟**内完成
- 失败后能**单独回滚**
- 有**明确完成标准**（不是"功能完成"，而是"X 测试通过"或"X 命令不报错"）

如果无法估计完成时间，说明粒度太大，需要继续拆分。

## 三种拆分维度

| 维度       | 方法                         |
| ---------- | ---------------------------- |
| 按功能层   | API → Service → DB 逐层拆分  |
| 按数据流   | 读 → 写 → 删 分开实现        |
| 按用户场景 | 主路径 → 异常 → 边界依次处理 |

## 禁止"顺手"原则

AI 会在完成任务时顺手重构相关代码。**每个任务只允许改任务要求的内容**，不允许顺手优化不相关部分。

需要在任务描述里明确写出：

> 只改 X，不改 Y 和 Z，即使你认为它们需要重构。

## 任务结构模板

```
任务标题
背景：为什么要做这件事
具体做什么：明确的行动项
完成标准：
 - [ ] 特定测试通过
 - [ ] 特定功能可演示
 - [ ] 特定命令不报错
依赖：依赖哪些其他任务（显式声明，不靠口头约定）
禁止：不允许改动的范围

```


## 依赖显式声明

任务之间的依赖关系必须显式写在任务描述里，不依赖口头约定或 AI 自行判断执行顺序。

## 与 to-issues 的关系

`task-breakdown` 是方法论，skills/to-issues/to-issues 是工具化实现。`to-issues` 生成的每个 Issue 应符合 `task-breakdown` 的粒度标准。

## 在工作流中的位置

```
to-prd → task-breakdown / to-issues → 实现 → 验证

```


## 相关

- skills/to-issues/to-issues — 工具化实现：把 PRD 拆成符合粒度标准的 Issue
- skills/to-prd/to-prd — 上游：提供拆分的输入
- concepts/spec-driven-development — 同样强调先定义完成标准

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`task-breakdown`

**本地文件**：`skills/task-breakdown/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/task-breakdown" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/task-breakdown" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/task-breakdown/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/task-breakdown/SKILL.original.md` 替换 `SKILL.md`。
