# test-driven-development (Superpowers)

> **Skill 文件**：SKILL.md · SKILL.original.md

Superpowers 的 TDD 强制执行 skill。**这是规定，不是建议。**

## 强制循环

```
RED → 写失败测试 → 确认测试失败
GREEN → 写最少代码通过测试 → 确认测试通过
REFACTOR → 重构，保持测试绿色

```


每一步都必须完成，不能跳过。

## 关键约束

- **发现代码在测试之前写** → 要求删掉代码，先写测试
- 测试必须先失败（RED）才能继续写实现
- 实现只写让测试通过的最少代码，不多写
- 重构阶段必须保持所有测试绿色

## 与 mattpocock/skills 的对比

vertical-slice-tdd 是 mattpocock 的 TDD 原则（一条测试紧跟一条实现，垂直切片）；Superpowers 的 `test-driven-development` 是**强制执行机制**——不只是原则，而是 AI 被约束必须遵守的流程。

## 反模式参考

skill 内置了 TDD 反模式列表，帮助 AI 识别并避免：

- 先写所有实现再补测试（伪 TDD）
- 测试只测 happy path
- 测试依赖外部状态（不可重复）
- 跳过 REFACTOR 阶段

## 相关

- entities/superpowers — Superpowers 完整介绍
- concepts/vertical-slice-tdd — TDD 的垂直切片原则
- skills/superpowers-verification-before-completion — 配套验证 skill
- skills/diagnose/diagnose — 调试时的对应 skill

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`tdd`

**本地文件**：`skills/superpowers-tdd/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/superpowers-tdd" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/superpowers-tdd" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/superpowers-tdd/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/superpowers-tdd/SKILL.original.md` 替换 `SKILL.md`。
