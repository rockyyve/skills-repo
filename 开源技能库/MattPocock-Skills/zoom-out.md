# zoom-out

mattpocock/skills 中的上下文扩展 skill。进入陌生模块时使用。

## 核心行为

让 agent 从当前局部代码**跳到更高抽象层**，解释：

- 相关模块是什么
- 谁在调用这段代码
- 领域概念如何组成整体地图

## 使用时机

- 进入陌生模块时
- 修改一段代码但不确定影响范围时
- 需要向新成员解释系统结构时

## 设计意图

`zoom-out` 是一个**定向的上下文请求**，而不是让 agent 自由探索。它强制 agent 先建立系统地图，再做局部修改，避免"只见树木不见森林"的修改。

## 相关

- concepts/architecture-decay — zoom-out 是对抗架构腐烂的认知工具
- skills/improve-codebase-architecture — 更深度的架构审查
- entities/mattpocock-skills — 仓库全貌
