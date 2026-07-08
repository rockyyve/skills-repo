# to-issues

> **Skill 文件**：SKILL.md · SKILL.original.md

mattpocock/skills 中的任务拆解 skill。把 PRD、计划或方案拆成可独立领取的 issue。

## 核心原则

强调**纵向切片**：每个任务都应是可演示、可验证、可独立完成的一条完整路径。反对水平切片（先把所有 model 写完，再写所有 controller）。

每个任务的标准结构：

```
1. 任务标题
2. 背景说明
3. 要实现什么
4. 验收标准
5. 依赖关系
6. 是否适合交给 agent 独立完成

```


## 非 GitHub 团队使用方式

`to-issues` 的真正价值是**拆解方法论**，不是 GitHub 集成。`setup-matt-pocock-skills` 支持配置多种输出目标：

- GitHub Issues（默认）
- GitLab Issues
- 本地 markdown 文件（`.scratch/<feature>/xxx.md`）
- Jira、Linear、飞书、Tapd、禅道、公司内部系统

对于非 GitHub 团队，可以把输出复制到任意工具，或直接写成本地 markdown。核心方法论不变。

## 在工作流中的位置

```
grill-with-docs → to-prd → [to-issues] → tdd / diagnose

```


前置：skills/to-prd/to-prd（需要先有 PRD）
后续：tdd、skills/diagnose/diagnose

## Issue 结构模板（古法编程 补充）

Issue 是**任务执行单元**，不是功能描述。完整结构：

```
背景：为什么要做（来自 PRD 的上下文）
任务：具体做什么
完成标准：
 - [ ] X 测试通过（不是"功能完成"）
 - [ ] X 命令不报错
不做的事：明确排除的范围

```


## Issue 粒度标准

- 工作量 **< 半天**
- 如果无法估计时间，说明粒度太大，继续拆分
- 每个 Issue 只做一件事
- 有明确依赖关系声明

## 验收标准的质量检查

生成 Issue 后检查完成标准是否**可验证**：

| 不合格         | 合格                       |
| -------------- | -------------------------- |
| "功能完成"     | "X 单元测试全部通过"       |
| "用户可以登录" | "`npm test auth` 无失败项" |
| "性能更好"     | "P99 响应时间 < 200ms"     |

## 相关

- skills/to-prd/to-prd — 生成 PRD 的上游 skill
- skills/task-breakdown/task-breakdown — 拆解方法论（粒度标准的理论基础）
- concepts/vertical-slice-tdd — 同样强调纵向切片的 TDD 原则
- entities/mattpocock-skills — 仓库全貌

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`to-issues`

**本地文件**：`skills/to-issues/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/to-issues" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/to-issues" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/to-issues/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/to-issues/SKILL.original.md` 替换 `SKILL.md`。
