# Documentation and ADRs（文档和架构决策记录）

## 概述

记录决策，不只是代码。最有价值的文档捕获 _why_——导致决策的上下文、约束和权衡。代码展示 _what_ 被构建；文档解释 _why 用这种方式构建_ 和 _考虑了什么替代方案_。

## 什么时候用

- 做重大架构决策
- 在竞争方案之间选择
- 添加或变更公共 API
- 发布改变用户面向行为的功能
- 新团队成员（或 Agent）上手项目
- 当你发现自己在重复解释同一件事

**什么时候不用：** 不要文档化显而易见的代码。不要添加重述代码本身的注释。

## 架构决策记录（ADR）

ADR 捕获重大技术决策背后的推理。它们是你能写的最高价值文档。

### 什么时候写 ADR

- 选择框架、库或主要依赖
- 设计数据模型或数据库 schema
- 选择认证策略
- 决定 API 架构（REST vs GraphQL vs tRPC）
- 在构建工具、托管平台或基础设施之间选择
- 任何回退代价很高的决策

### ADR 模板

存放在 `docs/decisions/` 中，顺序编号：

```markdown
# ADR-001: Use PostgreSQL for primary database

## Status

Accepted | Superseded by ADR-XXX | Deprecated

## Date

2025-01-15

## Context

[为什么需要做这个决策，关键需求]

## Decision

[做出了什么决定]

## Alternatives Considered

### [替代方案 1]

- Pros / Cons / Rejected 原因

## Consequences

[这个决定的后果]

```


### ADR 生命周期

```
PROPOSED → ACCEPTED → (SUPERSEDED or DEPRECATED)

```


- **不要删除旧 ADR。** 它们捕获历史上下文。
- 决策变更时，写新的 ADR 引用并取代旧的。

## 内联文档

### 什么时候写注释

注释 _why_，不注释 _what_：

```typescript
// BAD: 重述代码
counter += 1

// GOOD: 解释非明显意图
// Rate limit uses a sliding window — reset counter at window boundary
if (now - windowStart > WINDOW_SIZE_MS) {
 counter = 0
 windowStart = now
}

```


### 什么时候不写注释

```typescript
// 不注释自解释代码
function calculateTotal(items: CartItem[]): number {
 return items.reduce((sum, item) => sum + item.price * item.quantity, 0)
}

// 不留应该现在就做的 TODO
// TODO: add error handling ← 直接加

// 不留注释掉的代码
// const oldImplementation = () => { ... } ← 删了，git 有历史

```


### 记录已知陷阱

```typescript
/**
 * IMPORTANT: This function must be called before the first render.
 * If called after hydration, it causes a flash of unstyled content.
 *
 * See ADR-003 for the full design rationale.
 */
export function initializeTheme(theme: Theme): void {
 // ...
}

```


## API 文档

### 类型内联（TypeScript 首选）

```typescript
/**
 * Creates a new task.
 *
 * @param input - Task creation data (title required, description optional)
 * @returns The created task with server-generated ID and timestamps
 * @throws {ValidationError} If title is empty or exceeds 200 characters
 *
 * @example
 * const task = await createTask({ title: 'Buy groceries' });
 */
export async function createTask(input: CreateTaskInput): Promise<Task> {
 // ...
}

```


## README 结构

每个项目应有 README 覆盖：

```markdown
# Project Name

一段话描述项目做什么。

## Quick Start

1. Clone the repo
2. Install dependencies: `npm install`
3. Set up environment: `cp .env.example .env`
4. Run: `npm run dev`

## Commands

| Command | Description |
| --------------- | ---------------- |
| `npm run dev` | Start dev server |
| `npm test` | Run tests |
| `npm run build` | Production build |

## Architecture

项目结构和关键设计决策概述。链接到 ADR。

```


## 常见合理化

| 合理化说法 | 现实 |
| -------------------- | ----------------------------------------------------------------- |
| "代码自解释" | 代码展示 what。它不展示 why、什么替代方案被拒绝、或什么约束适用。 |
| "API 稳定后再写文档" | 当你文档化时 API 稳定得更快。文档是设计的第一个测试。 |
| "没人看文档" | Agent 看。未来工程师看。3 个月后的你也看。 |
| "ADR 是开销" | 10 分钟的 ADR 能防 6 个月后同一决策的 2 小时争论。 |

## 危险信号

- 架构决策没有书面推理
- 公共 API 没有文档或类型
- README 不解释如何运行项目
- 注释掉的代码代替删除
- 存在数周的 TODO 注释
- 有重大架构选择的项目没有 ADR

## 验证

文档化后：

- [ ] 所有重大架构决策有 ADR
- [ ] README 覆盖快速开始、命令和架构概述
- [ ] API 函数有参数和返回类型文档
- [ ] 已知陷阱在相关位置内联记录
- [ ] 没有注释掉的代码
- [ ] 规则文件（CLAUDE.md 等）是最新的
