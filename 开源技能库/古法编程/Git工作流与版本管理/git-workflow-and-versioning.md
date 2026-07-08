# git-workflow-and-versioning (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 改完代码怎么整理提交"的问题。

## AI commit 的常见问题

AI 写完代码后的 commit 往往**粒度太粗**（一个 commit 包含多个不相关改动）或**太细**（每行都是一个 commit）。两种情况都难以理解和回滚。

## 好 commit 的标准

- **一个 commit 只做一件事**
- 能**独立理解**——不需要看上下文 commit
- 能**独立回滚**——revert 后不会破坏其他功能

## Commit Message 格式

```
<类型>(<范围>): <简洁描述>

[可选] 为什么做这个改动

```


类型：`feat` / `fix` / `refactor` / `test` / `docs` / `chore`

示例：

```
fix(auth): 修复 token 过期后没有跳转登录页的问题

旧逻辑在 401 时直接返回错误，用户看不到提示。

```


## 让 AI 帮整理 commit

```
这是我的 git diff。
帮我建议应该拆成几个 commit，每个 commit 包含哪些改动，用什么 message。
不要直接执行，先给我建议。

```


关键：让 AI 给建议，人来决定是否采纳，再手动执行。

## Feature Branch 工作流

- 每个功能/修复在独立分支开发
- 合并前做 code review（见 skills/code-review-gu-fa-bian-cheng/code-review-gu-fa-bian-cheng）
- main/master 分支只有通过 review 的代码

## 不要让 AI 直接 push

**commit 后人工检查 diff，确认内容正确再 push。** AI 可能在 commit 里包含不该提交的文件（.env、临时调试代码）。

## 关联

- skills/code-review-gu-fa-bian-cheng/code-review-gu-fa-bian-cheng — 合并前的代码审查
- skills/ci-cd-and-automation-gu-fa-bian-cheng/ci-cd-and-automation-gu-fa-bian-cheng — push 后的自动化质量检查
- skills/superpowers-using-git-worktrees — 使用 git worktree 并行开发

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`git-workflow-and-versioning`

**本地文件**：`skills/git-workflow-and-versioning-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/git-workflow-and-versioning-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/git-workflow-and-versioning-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/git-workflow-and-versioning-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/git-workflow-and-versioning-古法编程/SKILL.original.md` 替换 `SKILL.md`。
