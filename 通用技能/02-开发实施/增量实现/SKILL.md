# Incremental Implementation（增量实现）

## 概述

用薄纵向切片构建——实现一块、测试它、验证它，然后扩展。避免一次性实现整个功能。每个增量应该让系统处于可工作、可测试的状态。这是让大型功能可控的执行纪律。

## 什么时候用

- 实现任何多文件变更
- 从任务拆解构建新功能
- 重构已有代码
- 任何你忍不住在测试前写超过约 100 行代码的时候

**什么时候不用：** 范围已经最小的单文件、单函数变更。

## 增量循环

```
┌──────────────────────────────────────┐
│ │
│ Implement ──→ Test ──→ Verify ──┐ │
│ ▲ │ │
│ └───── Commit ◄─────────────┘ │
│ │ │
│ ▼ │
│ Next slice │
│ │
└──────────────────────────────────────┘

```


每个切片：

1. **实现**最小的完整功能片段
2. **测试** — 运行测试套件（如果没有测试就写一个）
3. **验证** — 确认切片按预期工作（测试通过、构建成功、手动检查）
4. **提交** — 用描述性消息保存进度（参见 `git-workflow-and-versioning`）
5. **进入下一切片** — 继续，不要重新开始

## 切片策略

### 纵向切片（首选）

构建一个贯穿技术栈的完整路径：

```
Slice 1: Create a task (DB + API + basic UI)
 → Tests pass, user can create a task via the UI

Slice 2: List tasks (query + API + UI)
 → Tests pass, user can see their tasks

Slice 3: Edit a task (update + API + UI)
 → Tests pass, user can modify tasks

Slice 4: Delete a task (delete + API + UI + confirmation)
 → Tests pass, full CRUD complete

```


每个切片交付可工作的端到端功能。

### 契约优先切片

当后端和前端需要并行开发时：

```
Slice 0: Define the API contract (types, interfaces, OpenAPI spec)
Slice 1a: Implement backend against the contract + API tests
Slice 1b: Implement frontend against mock data matching the contract
Slice 2: Integrate and test end-to-end

```


### 风险优先切片

先攻克最危险或最不确定的部分：

```
Slice 1: Prove the WebSocket connection works (highest risk)
Slice 2: Build real-time task updates on the proven connection
Slice 3: Add offline support and reconnection

```


如果 Slice 1 失败，你在投入 Slice 2 和 3 之前就能发现。

## 实现规则

### 规则 0：简洁优先

写任何代码之前先问："能工作的最简方案是什么？"

写完代码后对照这些检查：

- 能用更少行实现吗？
- 这些抽象对得起它们的复杂度吗？
- 高级工程师会说"你为什么不直接..."吗？
- 我是在为假设的未来需求构建，还是当前任务？

```
SIMPLICITY CHECK:
✗ 为一个通知搞 EventBus 和中间件管线
✓ 简单函数调用

✗ 为两个相似组件搞抽象工厂
✓ 两个直白组件加共享工具

✗ 为三个表单搞配置驱动的表单构建器
✓ 三个表单组件

```


三行相似的代码胜过一个过早的抽象。先实现朴素的、明显正确的版本。只有在正确性被测试证明后才优化。

### 规则 0.5：范围纪律

只碰任务需要的东西。

不要：

- "顺手清理"你修改旁边的代码
- 重构你没在修改的文件中的 import
- 删除你不完全理解的注释
- 添加规格中没有的功能因为"看起来有用"
- 在你只是阅读的文件中现代化语法

如果你注意到任务范围外值得改进的东西，记录它——不要修复：

```
NOTICED BUT NOT TOUCHING:
- src/utils/format.ts has an unused import (unrelated to this task)
- The auth middleware could use better error messages (separate task)
→ Want me to create tasks for these?

```


### 规则 1：一次一件事

每个增量只改变一个逻辑事项。不要混杂关注点：

**坏：** 一个提交添加新组件、重构已有组件、还更新构建配置。

**好：** 三个独立提交——每个变更一个。

### 规则 2：保持可编译

每个增量之后，项目必须构建成功，已有测试必须通过。不要让代码库在切片之间处于损坏状态。

### 规则 3：未完成功能用 Feature Flags

如果功能还没准备好但你需要合并增量：

```typescript
const ENABLE_TASK_SHARING = process.env.FEATURE_TASK_SHARING === 'true';

if (ENABLE_TASK_SHARING) {
 // New sharing UI
}

```


这样你可以把小增量合并到主分支而不暴露未完成的工作。

### 规则 4：安全默认值

新代码应默认安全、保守的行为：

```typescript
export function createTask(data: TaskInput, options?: { notify?: boolean }) {
 const shouldNotify = options?.notify ?? false;
 // ...
}

```


### 规则 5：可回滚

每个增量应可独立回滚：

- 增加性变更（新文件、新函数）容易回滚
- 对已有代码的修改应最小且聚焦
- 数据库迁移应有对应的回滚迁移
- 避免在同一提交中删除又替换——分开它们

## 与 Agent 协作

指导 Agent 增量实现时：

```
"Let's implement Task 3 from the plan.

Start with just the database schema change and the API endpoint.
Don't touch the UI yet — we'll do that in the next increment.

After implementing, run `npm test` and `npm run build` to verify
nothing is broken."

```


对每个增量明确什么是范围内的、什么不是。

## 增量检查清单

每个增量之后，验证：

- [ ] 变更做了一件事并完整做完
- [ ] 所有已有测试仍然通过（`npm test`）
- [ ] 构建成功（`npm run build`）
- [ ] 类型检查通过（`npx tsc --noEmit`）
- [ ] Lint 通过（`npm run lint`）
- [ ] 新功能按预期工作
- [ ] 变更已提交并附描述性消息

## 常见合理化

| 合理化说法 | 现实 |
|---|---|
| "我最后一起测" | Bug 会叠加。Slice 1 的 bug 让 Slice 2-5 全错。每个切片都测试。 |
| "一次性做更快" | 感觉更快，直到出问题了你找不到是 500 行改动中的哪行引起的。 |
| "这些改动太小不用分开提交" | 小提交是免费的。大提交隐藏 bug 并让回滚痛苦。 |
| "feature flag 以后再加" | 如果功能没完成，它不应该是用户可见的。现在就加。 |
| "这个重构很小可以一起放" | 重构混进功能让两者都更难审查和调试。分开它们。 |

## 危险信号

- 写了超过 100 行代码没有运行测试
- 一个增量中有多个不相关变更
- "让我顺手加上这个"范围膨胀
- 跳过测试/验证步骤以加快速度
- 增量之间构建或测试损坏
- 大量未提交变更累积
- 在第三个用例要求之前就构建抽象
- "顺手"修改任务范围外的文件
- 为一次性操作创建新工具文件

## 验证

完成一个任务的所有增量后：

- [ ] 每个增量都经过独立测试和提交
- [ ] 完整测试套件通过
- [ ] 构建干净
- [ ] 功能端到端按规格工作
- [ ] 没有未提交的变更
