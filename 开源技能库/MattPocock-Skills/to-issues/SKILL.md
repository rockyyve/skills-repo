# To Issues（转成 Issue）

来源：Matt Pocock Skills 原版，英文入口保留在 `SKILL.original.md`。

用 vertical slice，也就是 tracer bullet，把计划拆成可以独立领取的 issue。

Issue tracker 和 triage label 词汇应该已经提供给你；如果没有，先运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 收集上下文

从对话上下文中已有的内容开始。如果用户把 issue 引用作为参数传进来，例如 issue 编号、URL 或路径，就从 issue tracker 中获取它，并阅读完整正文和评论。

### 2. 探索代码库（可选）

如果还没有探索过代码库，就先探索，理解当前代码状态。Issue 标题和描述应该使用项目领域词汇表中的词汇，并尊重要触碰区域里的 ADR。

### 3. 起草 vertical slice

把计划拆成 **tracer bullet** issue。每个 issue 都是一条很薄的 vertical slice，端到端穿过**所有集成层**，而不是某一层的 horizontal slice。

Slice 可以是 HITL 或 AFK。HITL slice 需要人类交互，例如架构决策或设计审查。AFK slice 可以在没有人类交互的情况下实现并合并。能用 AFK 时优先 AFK。

<vertical-slice-rules>

- 每个 slice 都交付一条狭窄但完整的路径，穿过每一层（schema、API、UI、tests）。
- 完成的 slice 本身可以 demo 或验证。
- 多个薄 slice 优于少数厚 slice。
</vertical-slice-rules>

### 4. 询问用户

把建议拆分展示为编号列表。每个 slice 显示：

- **Title**：简短描述性名称。
- **Type**：HITL / AFK。
- **Blocked by**：必须先完成哪些其他 slice，如果有的话。
- **User stories covered**：覆盖哪些用户故事，如果来源材料中有。

询问用户：

- 颗粒度是否合适？太粗还是太细？
- 依赖关系是否正确？
- 是否有 slice 应该合并或进一步拆分？
- HITL 和 AFK 的标记是否正确？

持续迭代，直到用户批准拆分。

### 5. 发布 issue 到 issue tracker

对每个获批的 slice，在 issue tracker 中发布一个新 issue。使用下面的 issue body 模板。应用 `needs-triage` triage label，让每个 issue 进入正常 triage 流程。

按依赖顺序发布 issue，先发布 blocker，这样可以在 “Blocked by” 字段里引用真实 issue identifier。

<issue-template>

## Parent

父 issue 在 issue tracker 中的引用。如果来源本身不是现有 issue，就省略这一节。

## What to build

用简洁文字描述这条 vertical slice。描述端到端行为，不要按层描述实现。

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- blocking ticket 的引用，如果有的话。

如果没有 blocker，就写 “None - can start immediately”。

</issue-template>

不要关闭或修改任何 parent issue。
