# To PRD（转成 PRD）

来源：Matt Pocock Skills 原版，英文入口保留在 `SKILL.original.md`。

这个技能使用当前对话上下文和对代码库的理解来产出 PRD。不要重新采访用户，只综合你已经知道的内容。

Issue tracker 和 triage label 词汇应该已经提供给你；如果没有，先运行 `/setup-matt-pocock-skills`。

## 流程

1. 如果还没有探索过仓库，先探索仓库，理解代码库当前状态。在整份 PRD 中使用项目领域词汇表里的词汇，并尊重要触碰区域里的 ADR。

2. 梳理为了完成实现需要构建或修改的主要 module。主动寻找可以抽取 deep module 的机会，让它们能被隔离测试。

Deep module 相对于 shallow module，是指用简单、可测试、很少变化的 interface 封装大量功能的 module。

和用户确认这些 module 是否符合预期。和用户确认哪些 module 需要写测试。

3. 使用下面模板写 PRD，然后发布到项目 issue tracker。应用 `needs-triage` triage label，让它进入正常 triage 流程。

<prd-template>

## Problem Statement

从用户视角描述用户面临的问题。

## Solution

从用户视角描述问题的解决方案。

## User Stories

写一份很长的编号 user story 列表。每条 user story 使用这个格式：

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>

1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

这份 user story 列表应该非常完整，覆盖这个功能的所有方面。

## Implementation Decisions

列出已经做出的实现决策，可以包括：

- 会构建或修改哪些 module。
- 会修改这些 module 的哪些 interface。
- 来自开发者的技术澄清。
- 架构决策。
- Schema 变化。
- API contract。
- 具体交互。

不要包含具体文件路径或代码片段。它们很快就可能过期。

## Testing Decisions

列出已经做出的测试决策，包括：

- 什么是好测试：只测试外部行为，不测试实现细节。
- 哪些 module 会被测试。
- 测试的先例，也就是代码库里类似类型的测试。

## Out of Scope

描述这个 PRD 不包含哪些事情。

## Further Notes

关于该功能的其他备注。

</prd-template>
