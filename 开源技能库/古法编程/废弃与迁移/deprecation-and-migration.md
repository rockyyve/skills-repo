# deprecation-and-migration (古法编程)

> **Skill 文件**：SKILL.md · SKILL.original.md

解决"AI 做迁移怎么不把旧代码拖死"的问题。

## 迁移的核心风险

新旧代码并存期间，**两套代码都需要维护，复杂度翻倍**。时间拖得越长，维护成本越高，迁移也越难完成。

## 迁移策略：绞杀者模式（Strangler Fig）

不要大爆炸重写（Big Bang Rewrite）。

**绞杀者模式**：

- 新功能用新方案实现
- 旧代码按模块逐步替换
- 系统在任何时刻都保持可运行状态

这是业界公认的低风险迁移策略。

## 给 AI 的约束模板

```
每次只迁移一个模块。
迁移完后跑测试确认旧行为保留，再继续下一个。
不要同时动多个模块。

```


一次只动一个是关键约束。AI 倾向于一次性完成，人需要明确限制范围。

## 废弃标记规范

在旧代码上加废弃注释，让后来者知道不该用：

```python
# DEPRECATED: 使用 new_module.NewClass 代替
# 计划在 v3.0 删除

```


注释应包含：替代方案在哪里、计划何时删除。

## 保留旧接口

**在所有调用方迁移完成前，不要删除旧接口。** 即使旧接口已废弃，删除前必须确认零调用。

## 迁移完成标准

- 旧代码**完全删除**（不是注释掉）
- 测试全绿
- 没有对旧接口的调用（用 grep 或 LSP 引用查找确认）

## 关联

- skills/git-workflow-and-versioning-gu-fa-bian-cheng/git-workflow-and-versioning-gu-fa-bian-cheng — 迁移过程中的 commit 管理
- skills/ci-cd-and-automation-gu-fa-bian-cheng/ci-cd-and-automation-gu-fa-bian-cheng — 用 CI 检测是否有旧接口调用残留
- skills/improve-codebase-architecture/improve-codebase-architecture — 架构层面的渐进式改善

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`deprecation-and-migration`

**本地文件**：`skills/deprecation-and-migration-古法编程/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/deprecation-and-migration-古法编程" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/deprecation-and-migration-古法编程" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/deprecation-and-migration-古法编程/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/deprecation-and-migration-古法编程/SKILL.original.md` 替换 `SKILL.md`。
