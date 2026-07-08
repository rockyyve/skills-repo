# code-simplification (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 写复杂了怎么收回来"的问题。

## AI 写复杂的信号

- 函数超过 50 行
- 嵌套超过 3 层
- 变量名需要注释才能理解
- 同样的逻辑出现两次以上

## 核心原则

**简化不是重写。** 目标是让代码更容易读，而不是引入新抽象。重写会带来新风险；简化是在现有逻辑上降低认知负担。

## 三种简化策略

1. **提取函数** — 把做一件事的代码变成有名字的函数。名字本身就是文档。
2. **减少嵌套** — 用 early return 代替深层 if-else。条件越早退出，主路径越清晰。
3. **消除重复** — 同样逻辑出现两次就提取，但不要过早抽象（一次还不够）。

## 给 AI 的指令模板

```
不要改逻辑，只让这段代码更容易读。
告诉我你改了什么，为什么。

```


关键约束：明确禁止改逻辑，要求说明理由，防止 AI 借简化之名悄悄改行为。

## 测试保护

简化要有测试保护：

1. 简化前先确认有测试覆盖关键行为
2. 简化后立刻跑测试
3. 行为没变 = 简化成功

没有测试的简化是赌博。

## 反模式：为了简化而简化

如果读代码的人需要理解业务逻辑，有时候**显式比简洁更重要**。把 `if user.role == "admin" and user.is_active` 提取成 `can_manage()` 会隐藏意图，让新人不知道权限规则在哪里。

## 关联

- skills/code-review-gu-fa-bian-cheng/code-review-gu-fa-bian-cheng — 发现复杂代码的审查角度（可读性维度）
- skills/improve-codebase-architecture/improve-codebase-architecture — 架构层面的简化（deep modules 概念）

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`code-simplification`

**本地文件**：`skills/code-simplification-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/code-simplification-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/code-simplification-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/code-simplification-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/code-simplification-古法编程/SKILL.original.md` 替换 `SKILL.md`。
