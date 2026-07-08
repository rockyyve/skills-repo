# Grill with Docs（带文档拷问）

来源：Matt Pocock Skills 原版，英文入口保留在 `SKILL.original.md`。

<what-to-do>

围绕这个方案的每个方面持续追问我，直到我们达成共同理解。沿着设计决策树逐支走下去，一次解决一个决策之间的依赖关系。每个问题都给出你的推荐答案。

一次只问一个问题，等待反馈后再继续下一个。

如果某个问题可以通过探索代码库回答，就先探索代码库，而不是问我。

</what-to-do>

<supporting-info>

## 领域感知

探索代码库时，也要寻找已有文档。

### 文件结构

大多数仓库只有一个 context：

```text
/
├── CONTEXT.md
├── docs/
│ └── adr/
│ ├── 0001-event-sourced-orders.md
│ └── 0002-postgres-for-write-model.md
└── src/

```


如果根目录存在 `CONTEXT-MAP.md`，说明这个仓库有多个 context。这个 map 会指向每个 context 的位置：

```text
/
├── CONTEXT-MAP.md
├── docs/
│ └── adr/ ← 系统级决策
├── src/
│ ├── ordering/
│ │ ├── CONTEXT.md
│ │ └── docs/adr/ ← context 专属决策
│ └── billing/
│ ├── CONTEXT.md
│ └── docs/adr/

```


懒创建文件，只在确实有内容要写时创建。如果没有 `CONTEXT.md`，在第一个术语被确定时创建。如果没有 `docs/adr/`，在第一个 ADR 真正需要时创建。

## 会话中

### 对照 glossary 挑战说法

当用户使用的术语和 `CONTEXT.md` 里的既有语言冲突时，立刻指出来：“你的 glossary 把 cancellation 定义为 X，但你现在似乎在说 Y，到底是哪一个？”

### 锐化模糊语言

当用户使用模糊或过载术语时，提出一个精确的规范术语：“你在说 account，这里指 Customer 还是 User？它们不是一回事。”

### 讨论具体场景

讨论领域关系时，用具体场景做压力测试。主动构造能探测边界条件的场景，迫使用户精确定义概念之间的边界。

### 和代码交叉验证

当用户描述某个机制如何工作时，检查代码是否同意。如果发现矛盾，直接指出：“代码里取消的是整个 Order，但你刚才说可以部分取消，哪个才是对的？”

### 内联更新 CONTEXT.md

当一个术语被确定时，当场更新 `CONTEXT.md`。不要批量攒到最后。使用 [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md) 中的格式。

不要把 `CONTEXT.md` 和实现细节耦合在一起。只写对领域专家有意义的术语。

### 谨慎提出 ADR

只有三个条件都满足时，才提议创建 ADR：

1. **难以回滚**：以后改变主意的成本明显存在。
2. **没有上下文会令人意外**：未来读者会问“为什么这样做？”
3. **来自真实权衡**：确实有备选方案，而且你出于具体理由选择了其中一个。

三个条件少任何一个，就跳过 ADR。使用 [ADR-FORMAT.md](./ADR-FORMAT.md) 中的格式。

</supporting-info>
