# Planning and Task Breakdown（规划和任务拆解）

## 概述

把工作分解为小的、可验证的任务，并带明确的验收标准。好的任务拆解是 Agent 能可靠完成工作和产出一团乱麻之间的区别。每个任务应该小到能在一次聚焦会话中实现、测试和验证。

## 什么时候用

- 你有规格需要拆解为可实现的单元
- 任务太大或太模糊无法开始
- 工作需要跨多个 Agent 或会话并行化
- 你需要向人类沟通范围
- 实现顺序不明显

**什么时候不用：** 范围明显的单文件变更，或规格中已有定义良好的任务。

## 规划流程

### 步骤 1：进入计划模式

在写任何代码之前，处于只读模式：

- 读取规格和相关代码库部分
- 确定已有模式和规范
- 映射组件之间的依赖
- 记录风险和未知数

**规划期间不要写代码。** 输出是计划文档，不是实现。

### 步骤 2：识别依赖图

映射什么依赖什么：

```
Database schema
 │
 ├── API models/types
 │ │
 │ ├── API endpoints
 │ │ │
 │ │ └── Frontend API client
 │ │ │
 │ │ └── UI components
 │ │
 │ └── Validation logic
 │
 └── Seed data / migrations

```


实现顺序按依赖图从下往上：先构建基础。

### 步骤 3：纵向切片

不要先建完所有数据库，再建所有 API，再建所有 UI——一次构建一个完整的功能路径：

**坏做法（横向切片）：**

```
Task 1: Build entire database schema
Task 2: Build all API endpoints
Task 3: Build all UI components
Task 4: Connect everything

```


**好做法（纵向切片）：**

```
Task 1: User can create an account (schema + API + UI for registration)
Task 2: User can log in (auth schema + API + UI for login)
Task 3: User can create a task (task schema + API + UI for creation)
Task 4: User can view task list (query + API + UI for list view)

```


每个纵向切片交付可工作、可测试的功能。

### 步骤 4：写任务

每个任务遵循这个结构：

```markdown
## Task [N]: [简短描述性标题]

**Description:** 一段话解释这个任务完成什么。

**Acceptance criteria:**
- [ ] [具体的、可测试的条件]
- [ ] [具体的、可测试的条件]

**Verification:**
- [ ] Tests pass: `npm test -- --grep "feature-name"`
- [ ] Build succeeds: `npm run build`
- [ ] Manual check: [验证什么的描述]

**Dependencies:** [依赖的任务编号，或 "None"]

**Files likely touched:**
- `src/path/to/file.ts`
- `tests/path/to/test.ts`

**Estimated scope:** [Small: 1-2 files | Medium: 3-5 files | Large: 5+ files]

```


### 步骤 5：排序和检查点

按以下条件排列任务：

1. 依赖已满足（先建基础）
2. 每个任务完成后系统处于可工作状态
3. 每 2-3 个任务后有验证检查点
4. 高风险任务放前面（尽早失败）

添加明确检查点：

```markdown
## Checkpoint: After Tasks 1-3
- [ ] All tests pass
- [ ] Application builds without errors
- [ ] Core user flow works end-to-end
- [ ] Review with human before proceeding

```


## 任务大小指南

| 大小 | 文件数 | 范围 | 示例 |
|------|-------|------|------|
| **XS** | 1 | 单个函数或配置变更 | 添加验证规则 |
| **S** | 1-2 | 一个组件或端点 | 添加新 API 端点 |
| **M** | 3-5 | 一个功能切片 | 用户注册流程 |
| **L** | 5-8 | 多组件功能 | 带过滤和分页的搜索 |
| **XL** | 8+ | **太大——继续拆** | — |

如果是 L 或更大，应该拆成更小的任务。Agent 在 S 和 M 任务上表现最好。

**什么时候继续拆：**

- 需要不止一次聚焦会话（大约 2 小时以上的 Agent 工作）
- 验收标准无法用 3 个或更少的要点描述
- 涉及两个或更多独立子系统（如 auth 和 billing）
- 任务标题中写了"和"（说明这是两个任务）

## 计划文档模板

```markdown
# Implementation Plan: [Feature/Project Name]

## Overview
[一段话总结要构建什么]

## Architecture Decisions
- [Key decision 1 and rationale]
- [Key decision 2 and rationale]

## Task List

### Phase 1: Foundation
- [ ] Task 1: ...
- [ ] Task 2: ...

### Checkpoint: Foundation
- [ ] Tests pass, builds clean

### Phase 2: Core Features
- [ ] Task 3: ...
- [ ] Task 4: ...

### Checkpoint: Core Features
- [ ] End-to-end flow works

### Phase 3: Polish
- [ ] Task 5: ...
- [ ] Task 6: ...

### Checkpoint: Complete
- [ ] All acceptance criteria met
- [ ] Ready for review

## Risks and Mitigations
| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk] | [High/Med/Low] | [Strategy] |

## Open Questions
- [Question needing human input]

```


## 并行化机会

当有多个 Agent 或会话可用时：

- **可以并行的：** 独立功能切片、已实现功能的测试、文档
- **必须串行的：** 数据库迁移、共享状态变更、依赖链
- **需要协调的：** 共享 API 契约的功能（先定义契约再并行）

## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "我会边做边想" | 这就是你得到一团乱麻和返工的原因。10 分钟规划能省几小时。 |
| "任务很明显" | 还是写下来。明确的任务能暴露隐藏依赖和被遗忘的边界情况。 |
| "规划是开销" | 规划就是任务。没有计划的实现只是打字。 |
| "我能把所有东西都记在脑子里" | 上下文窗口是有限的。写下来的计划能跨越会话边界和压缩。 |

## 危险信号

- 没有书面任务列表就开始实现
- 任务写着"实现功能"但没有验收标准
- 计划中没有验证步骤
- 所有任务都是 XL 大小
- 任务之间没有检查点
- 没有考虑依赖顺序

## 验证

在开始实现之前，确认：

- [ ] 每个任务都有验收标准
- [ ] 每个任务都有验证步骤
- [ ] 任务依赖已正确识别和排序
- [ ] 没有任务修改超过约 5 个文件
- [ ] 主要阶段之间有检查点
- [ ] 人类已审查并批准计划
