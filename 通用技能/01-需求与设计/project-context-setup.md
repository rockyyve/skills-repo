# project-context-setup

解决的核心问题：AI 每次对话都要重新理解项目背景，导致歧义、用错术语、忽略已有决策。通过建立三类核心文件，让 AI 用项目自己的语言工作。

## 三类核心文件

### CONTEXT.md — 领域词汇表

内容：

- 项目是什么、给谁用、解决什么问题
- **核心领域词汇和定义**（最重要）

示例词汇条目：

```
Customer: 签约付费的企业主体
User: Customer 下的具体操作人员
Account: 可能指 Customer 账户或 User 账户，需看上下文

```


### ARCHITECTURE.md — 系统结构

内容：

- 系统组件及各组件职责
- 组件间关系
- 数据流向

### docs/adr/ — 架构决策记录（ADR）

每条 ADR 记录：

- 做了什么决定
- 为什么这样决定
- 放弃了哪些方案（以及原因）

ADR 防止 AI 在新功能里重新引入已被否决的方案。

## 启动对话的用法

每次新对话开始时，先给 AI 读这几个文件：

```
请先读以下文件再开始：
- CONTEXT.md
- ARCHITECTURE.md
- docs/adr/（相关决策）

```


效果：减少歧义、让 AI 用项目语言回答、避免违反已有架构决策。

## 维护原则

这些文件要**随代码一起提交**，不是一次性文档。每次做了新的架构决策，立即更新 ADR；新增领域概念，立即更新 CONTEXT.md。

## 与 grill-with-docs 的关系

skills/grill-with-docs 依赖 `project-context-setup` 建立的这些文件来进行精准追问。没有 CONTEXT.md，`grill-with-docs` 就退化成普通追问。

## 在工作流中的位置

```
project-context-setup（一次性建立）
 ↓
grill-with-docs → to-prd → to-issues → 实现

```


## 相关

- skills/grill-with-docs — 使用这些文件进行精准需求追问
- concepts/spec-driven-development — 同样强调在写代码前建立共同语言
- skills/idea-refine — 通常在 project-context-setup 之后、grill-with-docs 之前
