# Test-Driven Development（测试驱动开发）

来源：Matt Pocock Skills 原版，英文入口保留在 `SKILL.original.md`。

## 哲学

**核心原则**：测试应该通过 public interface 验证行为，而不是验证实现细节。代码可以完全改变，测试不应该跟着坏。

**好测试**是 integration-style：它们通过 public API 走真实代码路径。它们描述系统做了什么，而不是怎么做。一个好测试读起来像规格说明——“user can checkout with valid cart” 会清楚说明存在什么能力。这类测试能经受重构，因为它们不关心内部结构。

**坏测试**和实现耦合。它们 mock 内部 collaborator、测试 private method，或通过外部手段验证，例如直接查询数据库，而不是使用 interface。警告信号：你重构时测试坏了，但行为并没有改变。如果你重命名内部函数后测试失败，那些测试测的是实现，不是行为。

示例见 [tests.md](tests.md)，mock 规则见 [mocking.md](mocking.md)。

## 反模式：横向切片

**不要先写所有测试，再写所有实现。** 这是 “horizontal slicing”：把 RED 理解成“写完全部测试”，把 GREEN 理解成“写完全部代码”。

这会产出**糟糕测试**：

- 批量写出来的测试测的是想象中的行为，不是真实行为。
- 最后测的是东西的形状，例如数据结构、函数签名，而不是用户可见行为。
- 测试会对真实变化不敏感：行为坏了它们仍通过，行为没坏它们却失败。
- 你会冲到视野之外，在理解实现前就提前承诺测试结构。

**正确做法**：用 tracer bullet 做 vertical slice。一个测试 → 一个实现 → 重复。每个测试都回应上一轮学到的东西。因为你刚写完代码，你清楚哪些行为重要，以及如何验证它。

~~~text
错误做法（horizontal）：
 RED: test1, test2, test3, test4, test5
 GREEN: impl1, impl2, impl3, impl4, impl5

正确做法（vertical）：
 RED→GREEN: test1→impl1
 RED→GREEN: test2→impl2
 RED→GREEN: test3→impl3
 ...
~~~

## 工作流

### 1. 规划

探索代码库时，使用项目的领域词汇表，让测试名和 interface 词汇匹配项目语言，并尊重要触碰区域里的 ADR。

写任何代码之前：

- [ ] 和用户确认需要哪些 interface 变化。
- [ ] 和用户确认要测试哪些行为，并排序。
- [ ] 识别 [deep modules](deep-modules.md) 的机会，也就是小 interface、深 implementation。
- [ ] 为 [testability](interface-design.md) 设计 interface。
- [ ] 列出要测试的行为，不列实现步骤。
- [ ] 获得用户对计划的确认。

提问：“Public interface 应该长什么样？哪些行为最重要，最需要测试？”

**你无法测试所有东西。** 和用户确认最重要的行为。把测试精力放在关键路径和复杂逻辑上，而不是每个可能的边界条件。

### 2. Tracer Bullet

写一个测试，只确认系统的一件事：

~~~text
RED: 为第一个行为写测试 → 测试失败
GREEN: 写刚好能通过的最小代码 → 测试通过
~~~

这就是 tracer bullet：证明路径能端到端跑通。

### 3. 增量循环

对剩下每个行为：

~~~text
RED: 写下一个测试 → 失败
GREEN: 写刚好能通过的最小代码 → 通过
~~~

规则：

- 一次只写一个测试。
- 只写当前测试需要的最小代码。
- 不预判未来测试。
- 让测试聚焦可观察行为。

### 4. 重构

所有测试通过后，寻找 [refactor candidates](refactoring.md)：

- [ ] 提取重复。
- [ ] 深化 module，把复杂度移到简单 interface 后面。
- [ ] 在自然的位置应用 SOLID 原则。
- [ ] 思考新代码暴露了现有代码的什么问题。
- [x] 每一步重构后都运行测试。 ✅ 2026-05-17

**RED 状态下永远不要重构。** 先回到 GREEN。

## 每轮检查清单

~~~text
[ ] 测试描述行为，而不是实现
[ ] 测试只使用 public interface
[ ] 测试能经受内部重构
[ ] 代码只够当前测试通过
[ ] 没有添加投机性功能
~~~
