# documentation-and-adrs (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 写文档怎么记录为什么"的问题。

## 最有价值的文档

**记录为什么做了某个决定，而不是记录代码做了什么。**

代码本身已经说明了"做了什么"。真正稀缺的是"为什么"——为什么选这个方案、为什么放弃了另一个、当时有哪些约束。

## ADR（架构决策记录）格式

```markdown
# ADR-001: [决定的标题]

## 背景

[当时面临什么问题或约束]

## 决定

[做了什么决定]

## 原因

[为什么选这个方案]

## 放弃的方案

[考虑过哪些备选方案，为什么没选]

## 后果

[这个决定带来了什么，包括缺点]

```


## 让 AI 写 ADR

```
我刚做了一个决定：[描述决定]。
帮我生成一个 ADR。
我会补充"原因"和"放弃的方案"部分，你先把框架填好。

```


人工补充原因和放弃的方案——这两部分最有价值，也是 AI 无法凭空生成的。

## 代码注释原则

**注释 why，不注释 what。**

```python
# Bad: 遍历用户列表（代码已经说了这件事）
for user in users:

# Good: 跳过未激活用户，避免触发付费功能的权限检查
for user in active_users:

```


## 文档要随代码提交

文档和代码分离会导致文档快速过时。ADR、注释、README 的更新应该和相关代码改动在同一个 commit 里。

## README 最低要求

1. 项目是什么（一句话）
2. 怎么运行（命令）
3. 怎么测试（命令）
4. 关键决定在哪里查（指向 ADR 目录）

## 关联

- skills/improve-codebase-architecture/improve-codebase-architecture — 架构改动应该配套 ADR
- skills/git-workflow-and-versioning-gu-fa-bian-cheng/git-workflow-and-versioning-gu-fa-bian-cheng — 文档随代码提交的 commit 规范

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`documentation-and-adrs`

**本地文件**：`skills/documentation-and-adrs-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/documentation-and-adrs-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/documentation-and-adrs-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/documentation-and-adrs-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/documentation-and-adrs-古法编程/SKILL.original.md` 替换 `SKILL.md`。
