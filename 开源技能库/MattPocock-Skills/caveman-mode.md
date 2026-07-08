# Caveman Mode (Terse AI Output)

mattpocock/skills 中名字最有梗的 skill：**穴居人模式**。

相关：entities/matt-pocock · entities/mattpocock-skills · concepts/vibe-coding · concepts/ai-coding-pain-points · concepts/ubiquitous-language

## 一句话定位

Agent 直接把废话、客气话、解释、过渡词全砍掉，只输出**技术结论**。长任务 token 占用立省 **~75%**。

## 何时用

- 开长会议（multi-hour session）
- 跑批量任务（大批量重命名、批量重构）
- 需要快速浏览许多候选答案
- 跑在便宜模型上，但希望输出更密

## 何时不用

- 教学场景（你需要解释）
- 复杂决策（你需要看到推理链）
- 不熟悉的代码库（你需要 Agent 多解释上下文）

## 它在透露什么

文章作者评论：

> `caveman` 的存在透露了什么？它说明 Matt 真的在乎 token 成本。一个连"穴居人模式"都开发出来的人，对工程实用主义的关注是写在 DNA 里的。

这条 skill 本身可能不是最有用的，但**它的存在**是一个信号：entities/mattpocock-skills 整套设计都偏向工程实用主义，不是炫技。

## 和 ubiquitous language 的协同

Caveman 让单条输出更短；ubiquitous language 让每个词更密。两者一起用：

| 场景              | 普通     | Caveman  | Caveman + UL |
| ----------------- | -------- | -------- | ------------ |
| 描述 1 个领域行为 | 80 token | 30 token | 5 token      |

5 个 token 比如：`materialization cascade broken` 已经说清楚"课程章节生成时的物理化流程出问题了"。

## 实操建议

文章作者把它列入"装上忘掉"清单——长期开着，不会出错。

> `caveman` + `git-guardrails` 长期开着。这俩装上忘掉就行。

但如果你在做需要解释的工作（onboarding、code review、复杂 debug 的报告环节），临时切回普通模式更稳。

## 相关 skill

- `git-guardrails-claude-code` — 另一个"装上忘掉"型 skill，在 hook 层拦 `push --force` / `reset --hard` / `clean -fd`。
- `/grill-with-docs` — 见 skills/grill-the-user，写代码前的拷问；和 caveman 互补（拷问要细致，干活要简洁）。
