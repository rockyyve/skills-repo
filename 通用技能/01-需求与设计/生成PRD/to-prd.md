# to-prd

> **Skill 文件**：SKILL.md · SKILL.original.md

mattpocock/skills 中的 PRD 生成 skill。把当前对话和代码理解综合成 PRD，并发布到项目 issue tracker。

## 核心行为

**不再采访用户**——这是与 grill-me / `grill-with-docs` 的关键区别。`to-prd` 假设需求已经澄清，它的工作是**整理**：

- 已知上下文
- 受影响的模块
- 用户故事
- 实现决策
- 测试决策

## 在工作流中的位置

```
grill-with-docs → [to-prd] → to-issues → tdd / diagnose

```


前置：`grill-with-docs` 或 `grill-me`（需求已澄清）
后续：skills/to-issues/to-issues（把 PRD 拆成任务）

## 设计意图

PRD 是**会话的凝结物**，不是文档工作。它把对话中散落的决策固化成一个可引用的 artifact，让后续的 `to-issues`、`tdd`、`handoff` 都有稳定的上下文锚点。

## PRD vs Spec 的区别

|          | Spec               | PRD                    |
| -------- | ------------------ | ---------------------- |
| 定位     | 沟通边界协议       | 执行规格               |
| 正式程度 | 轻量               | 更正式                 |
| 内容侧重 | 用户/成功标准/不做 | 背景 + 约束 + 功能范围 |

Spec 是早期对话工具，PRD 是进入执行前的凝结物。

## 触发时机

经过多轮对话澄清了需求、准备进入任务拆解之前触发 `to-prd`。

## PRD 生成 Prompt 模式

```
把这段对话整理成 PRD，包含：
- 背景
- 用户和场景
- 功能范围
- 不做的事
- 成功标准
- 技术约束

```


## 生成后验证

PRD 生成后逐条检查是否和对话一致，补充遗漏的约束。不要直接进入下一步，先确认 PRD 准确反映了所有澄清结果。

## 相关

- skills/to-issues/to-issues — 下游：把 PRD 拆成任务
- skills/grill-with-docs/grill-with-docs — 上游：需求拷问
- skills/grill-the-user — 上游：需求拷问（另一变体）
- entities/mattpocock-skills — 仓库全貌
- skills/task-breakdown/task-breakdown — 拆解方法论，与 to-issues 配合使用

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`to-prd`

**本地文件**：`skills/to-prd/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/to-prd" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/to-prd" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/to-prd/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/to-prd/SKILL.original.md` 替换 `SKILL.md`。
