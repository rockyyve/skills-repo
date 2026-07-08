# idea-refine

> **Skill 文件**：SKILL.md · SKILL.original.md

解决的核心问题：想法还很散就让 AI 开始写代码。`idea-refine` 不是让 AI 替你拍板，而是通过提问、发散、收敛，把一个模糊念头变成有用户、有成功标准、有 MVP 边界的具体方向。

适合放在写 spec 前面，解决"这事值不值得写、先写哪个版本"的问题。

## 三阶段流程

### 1. 澄清阶段

先不进入技术设计，优先问清楚：

- **用户和场景**：具体给谁用、用户现在怎么解决这个问题
- **成功标准**：成功是什么样子（可验证）
- **资源限制**：时间、人力、技术约束
- **时机判断**：为什么现在做

**Prompt 模式**：

> 先不要写实现方案，先把想法改写成明确问题陈述，问 3-5 个必须澄清的问题，确认用户/场景/成功标准前不进入技术设计。

### 2. 发散阶段

生成 5-8 个方向，每个方向必须有**不同假设**，而非同一想法换名字。

### 3. 收敛阶段

逐步过滤：

- 哪个方向最像刚需
- 哪个版本最容易验证
- 哪些假设风险最大
- 哪些功能不做也能证明核心价值

## "Not Doing" 列表

收敛阶段必须明确列出**暂时不做**的内容。这一步防止 AI 所有好点子都塞进初版实现。

## 输出格式

一页纸结构：

```
1. 问题陈述（一句话）
2. 推荐方向
3. 关键假设
4. MVP 范围
5. 暂时不做

```


## 在工作流中的位置

```
idea-refine → spec-driven-development → grill-with-docs → to-prd → to-issues

```


## 相关

- concepts/spec-driven-development — 下游：把方向变成结构化规格
- skills/grill-with-docs/grill-with-docs — 下游：带项目上下文的需求澄清
- skills/to-prd/to-prd — 更下游：把对话整理成 PRD

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`idea-refine`

**本地文件**：`skills/idea-refine/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/idea-refine" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/idea-refine" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/idea-refine/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/idea-refine/SKILL.original.md` 替换 `SKILL.md`。
