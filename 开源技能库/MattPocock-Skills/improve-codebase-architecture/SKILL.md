# Improve Codebase Architecture（改进代码库架构）

来源：Matt Pocock Skills 原版，英文入口保留在 `SKILL.original.md`。

找出架构摩擦，并提出 **deepening opportunity**：把 shallow module 重构成 deep module。目标是提高可测试性和 AI 可导航性。

## 术语表

在每条建议里都准确使用这些术语。保持语言一致就是重点。不要漂移到 “component”“service”“API”“boundary”。完整定义见 [LANGUAGE.md](LANGUAGE.md)。

- **Module**：任何有 interface 和 implementation 的东西，可以是 function、class、package、slice。
- **Interface**：调用者为了使用 module 必须知道的一切：类型、invariant、错误模式、顺序、配置。不只是类型签名。
- **Implementation**：内部代码。
- **Depth**：interface 上的杠杆。小 interface 后面藏着大量行为。**Deep** = 高杠杆。**Shallow** = interface 和 implementation 一样复杂。
- **Seam**：interface 所在的位置；可以不在原地编辑就改变行为的地方。使用这个词，不用 “boundary”。
- **Adapter**：在某个 seam 上满足 interface 的具体东西。
- **Leverage**：调用者从 depth 中获得的收益。
- **Locality**：维护者从 depth 中获得的收益：变更、Bug、知识集中在一个地方。

关键原则见 [LANGUAGE.md](LANGUAGE.md) 的完整列表：

- **删除测试**：想象删除这个 module。如果复杂度消失了，它只是 pass-through。如果复杂度重新出现在 N 个调用方，它就在发挥作用。
- **Interface 就是测试面。**
- **一个 adapter = 假想 seam。两个 adapter = 真实 seam。**

这个技能会被项目的领域模型影响。领域语言会给好的 seam 命名；ADR 会记录不应该重复争论的决策。

## 流程

### 1. 探索

先阅读项目的领域词汇表，以及你要触碰区域里的 ADR。

然后使用 Agent tool，并设置 `subagent_type=Explore` 来遍历代码库。不要死套固定启发式；自然探索，并记录你在哪里感到摩擦：

- 理解一个概念时，是否需要在很多小 module 之间来回跳？
- 哪些 module 是 **shallow**，也就是 interface 几乎和 implementation 一样复杂？
- 哪些纯函数只是为了测试被抽出来，但真正的 Bug 藏在它们被调用的方式里，缺少 **locality**？
- 哪些紧耦合 module 会跨 seam 泄漏细节？
- 代码库哪些部分没有测试，或很难通过现有 interface 测试？

对任何你怀疑 shallow 的东西应用**删除测试**：删除它会集中复杂度，还是只是移动复杂度？“会集中”才是你想要的信号。

### 2. 展示候选项

展示一个编号列表，列出 deepening opportunity。每个候选项包含：

- **Files**：涉及哪些文件或 module。
- **Problem**：当前架构为什么造成摩擦。
- **Solution**：用普通语言说明会改变什么。
- **Benefits**：用 locality 和 leverage 解释收益，也说明测试会如何变好。

**领域部分使用 CONTEXT.md 的词汇，架构部分使用 [LANGUAGE.md](LANGUAGE.md) 的词汇。** 如果 `CONTEXT.md` 定义了 “Order”，就说 “Order intake module”，不要说 “FooBarHandler”，也不要说 “Order service”。

**ADR 冲突**：如果某个候选项和已有 ADR 矛盾，只有当摩擦真实到值得重开 ADR 时才提出。明确标记，例如：“contradicts ADR-0007 — but worth reopening because…”。不要列出每个被 ADR 禁止的理论重构。

现在不要提出 interface。先问用户：“这些里面你想探索哪一个？”

### 3. 拷问循环

用户选中候选项后，进入拷问式对话。和他们一起走设计树：约束、依赖、deepened module 的形状、seam 后面放什么、哪些测试会留下。

决策逐渐成形时，副作用要内联发生：

- **如果一个 deepened module 用了 `CONTEXT.md` 里没有的概念命名**：把这个术语加入 `CONTEXT.md`，纪律和 `/grill-with-docs` 相同，格式见 [CONTEXT-FORMAT.md](../grill-with-docs/CONTEXT-FORMAT.md)。如果文件不存在，懒创建。
- **对话中锐化了一个模糊术语**：当场更新 `CONTEXT.md`。
- **用户用一个关键理由拒绝了候选项**：提议创建 ADR，措辞是：“要不要把这个记录成 ADR，这样未来架构审查不会再次建议同一件事？” 只有当这个理由真的能帮助未来探索者避免重复建议时才提。短期理由和显而易见的理由不要提。
- **想探索 deepened module 的替代 interface**：参见 [INTERFACE-DESIGN.md](INTERFACE-DESIGN.md)。
