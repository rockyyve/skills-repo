# prototype

mattpocock/skills 中的原型 skill。目标是**快速验证设计选择**，原型本身是可丢弃的。

## 两种模式

| 问题类型           | 原型形式                      |
| ------------------ | ----------------------------- |
| 逻辑或状态模型问题 | 终端交互原型（CLI REPL 风格） |
| UI 设计问题        | 多版本界面原型，并排比较      |

## 设计意图

原型不是 MVP，不是要上线的代码。它的唯一目的是**回答一个设计问题**，然后被丢掉。

这与 TDD 的关系：原型用于验证"做什么"，TDD 用于验证"做对了没有"。两者不冲突，原型在 TDD 之前。

## 相关

- entities/mattpocock-skills — 仓库全貌
- concepts/vertical-slice-tdd — 原型之后的实现方法论
- skills/to-prd/SKILL — 原型结论可以沉淀进 PRD
