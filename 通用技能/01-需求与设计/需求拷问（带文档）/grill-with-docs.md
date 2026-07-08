# grill-with-docs

> **Skill 文件**：SKILL.md · SKILL.original.md

解决的核心问题：AI 写代码前听不懂项目语言。这不是普通追问需求，而是**带着项目上下文文档**一起追问，确保 AI 用项目自己的语言理解需求。

## 与普通追问的区别

| 普通问法 | grill-with-docs |
| ---------------------- | --------------------------------------------------------------------- |
| "这个功能有哪些边界？" | "CONTEXT.md 里 Customer 和 User 是两个概念，你这次说的账户指哪一个？" |
| 依赖 AI 自行理解术语 | 显式引用项目文档中的定义 |
| 口头共识 | 每次澄清形成书面记录 |

## 三类参考文档

1. **CONTEXT.md** — 领域词汇表，定义项目专有名词
2. **docs/adr/** — 已有架构决策记录，防止方案与过去决策冲突
3. **相关代码** — 验证术语在实际代码中的用法

## 追问流程

```
1. 先读 CONTEXT.md
2. 列出方案里出现的领域词
3. 逐个确认词义边界（引用 CONTEXT.md 定义）
4. 检查方案与 ADR 有无冲突
5. 修正方案后再开工

```


## 适用场景

- 新功能涉及多个领域概念
- 方案可能和已有 ADR 冲突
- AI 对领域术语用法不稳定（同一个词在不同回复里含义不同）

## 书面记录原则

每次澄清必须形成书面记录，不依赖口头共识。澄清结果可以更新 CONTEXT.md 或作为 PRD 的前置输入。

## 在工作流中的位置

```
idea-refine → grill-with-docs → to-prd → to-issues

```


与 skills/to-prd/to-prd 的关系：`grill-with-docs` 是采访阶段，`to-prd` 是整理阶段——前者澄清，后者凝结。

## 相关

- skills/project-context-setup — 前置：建立 CONTEXT.md 等项目文档
- skills/to-prd/to-prd — 下游：把澄清后的需求整理成 PRD
- concepts/spec-driven-development — 同样强调在写代码前消除歧义

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`grill-with-docs`

**本地文件**：`skills/grill-with-docs/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/grill-with-docs" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/grill-with-docs" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/grill-with-docs/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/grill-with-docs/SKILL.original.md` 替换 `SKILL.md`。
