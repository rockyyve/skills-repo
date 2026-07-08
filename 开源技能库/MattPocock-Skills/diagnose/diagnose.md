# diagnose

> **Skill 文件**：SKILL.md · SKILL.original.md

mattpocock/skills 中的调试 skill。适合难复现 bug 和性能回退。

## 6 阶段流程

1. **建立可重复反馈环** — 先有一个 2 秒能跑完的 pass/fail 命令（见 concepts/feedback-loop-driven-debugging）
2. **复现** — 复现率 < 50% 就不要试图修
3. **提出 3–5 个假设** — 定向而不是随机插桩
4. **定向插桩** — 只在假设指向的地方加 log/断点
5. **修复**
6. **补回归测试 + 清理调试代码**

## 关键约束

- 第一步必须是建立反馈环，不能跳过
- 复现率 < 50% → 停止，先解决复现问题
- 修复后必须清理所有调试代码

## 与 TDD 的关系

`diagnose` 是 bug 修复路径，tdd 是新功能路径。两者都要求先有反馈环，但 `diagnose` 的反馈环是"能复现 bug 的命令"，`tdd` 的反馈环是"能失败的测试"。

## 相关

- concepts/feedback-loop-driven-debugging — 反馈环的核心概念
- concepts/vertical-slice-tdd — 新功能的对应方法论
- entities/mattpocock-skills — 仓库全貌

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`diagnose`

**本地文件**：`skills/diagnose/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/diagnose" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/diagnose" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/diagnose/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/diagnose/SKILL.original.md` 替换 `SKILL.md`。
