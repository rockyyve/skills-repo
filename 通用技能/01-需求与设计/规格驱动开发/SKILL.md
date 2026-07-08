# Spec-Driven Development（规格驱动开发）

## 概述

在写任何代码之前先写结构化规格。规格是你和人类工程师之间的共享真相来源——它定义了要构建什么、为什么、以及怎么知道做完了。没有规格的代码就是在猜。

## 什么时候用

- 开始新项目或新功能
- 需求模糊或不完整
- 变更涉及多个文件或模块
- 你即将做一个架构决策
- 任务实现需要超过 30 分钟

**什么时候不用：** 单行修复、错字修正、或需求明确且自包含的变更。

## 门控工作流

规格驱动开发有四个阶段。当前阶段没有验证之前不要进入下一阶段。

```
SPECIFY ──→ PLAN ──→ TASKS ──→ IMPLEMENT
 │ │ │ │
 ▼ ▼ ▼ ▼
 人类 人类 人类 人类
 审查 审查 审查 审查

```


### 阶段 1：Specify（明确）

从高层愿景开始。向人类提出澄清问题，直到需求具体化。

**立即暴露假设。** 在写任何规格内容之前，列出你的假设：

```
ASSUMPTIONS I'M MAKING:
1. This is a web application (not native mobile)
2. Authentication uses session-based cookies (not JWT)
3. The database is PostgreSQL (based on existing Prisma schema)
4. We're targeting modern browsers only (no IE11)
→ Correct me now or I'll proceed with these.

```


不要默默填补模糊需求。规格的全部目的就是在代码写出来之前暴露误解——假设是最危险的误解形式。

**写一份规格文档，覆盖这六个核心领域：**

1. **目标** — 我们在构建什么、为什么？用户是谁？成功是什么样的？

2. **命令** — 完整可执行命令（含标志），不只是工具名。

 ```
 Build: npm run build
 Test: npm test -- --coverage
 Lint: npm run lint --fix
 Dev: npm run dev
 ```

3. **项目结构** — 源代码在哪、测试在哪、文档在哪。

 ```
 src/ → Application source code
 src/components → React components
 src/lib → Shared utilities
 tests/ → Unit and integration tests
 e2e/ → End-to-end tests
 docs/ → Documentation
 ```

4. **代码风格** — 一个真实的代码片段展示你的风格，比三段描述更有效。包含命名规范、格式规则和好的输出示例。

5. **测试策略** — 用什么框架、测试放哪、覆盖率期望、哪级测试覆盖哪些关注点。

6. **边界** — 三级系统：
 - **始终执行：** 提交前跑测试、遵循命名规范、验证输入
 - **先问：** 数据库 schema 变更、添加依赖、改 CI 配置
 - **绝不做：** 提交密钥、编辑 vendor 目录、未经批准删除失败测试

**规格模板：**

```markdown
# Spec: [Project/Feature Name]

## Objective

[在构建什么和为什么。用户故事或验收标准。]

## Tech Stack

[框架、语言、主要依赖及版本]

## Commands

[Build、test、lint、dev — 完整命令]

## Project Structure

[目录布局及描述]

## Code Style

[示例代码 + 关键规范]

## Testing Strategy

[框架、测试位置、覆盖率要求、测试级别]

## Boundaries

- Always: [...]
- Ask first: [...]
- Never: [...]

## Success Criteria

[怎么知道做完了——具体、可测试的条件]

## Open Questions

[任何需要人类输入的未解决问题]

```


**把指令重构成成功标准。** 收到模糊需求时，把它们转译成具体条件：

```
REQUIREMENT: "Make the dashboard faster"

REFRAMED SUCCESS CRITERIA:
- Dashboard LCP < 2.5s on 4G connection
- Initial data load completes in < 500ms
- No layout shift during load (CLS < 0.1)
→ Are these the right targets?

```


这样你就能循环、重试、朝一个清晰目标解决问题，而不是猜"更快"是什么意思。

### 阶段 2：Plan（规划）

有了验证过的规格，生成技术实现计划：

1. 确定主要组件和依赖关系
2. 确定实现顺序（必须先构建什么）
3. 记录风险和缓解策略
4. 确定哪些可以并行构建、哪些必须串行
5. 在阶段之间定义验证检查点

计划应该是可审查的：人类能读它然后说"是的，这是正确的方案"或"不，改 X"。

### 阶段 3：Tasks（任务拆解）

把计划拆成离散的、可实现的任务：

- 每个任务应能在一次聚焦会话中完成
- 每个任务有明确的验收标准
- 每个任务包含验证步骤（测试、构建、手动检查）
- 按依赖排序，不按重要性
- 任何任务不应修改超过约 5 个文件

**任务模板：**

```markdown
- [ ] Task: [Description]
 - Acceptance: [What must be true when done]
 - Verify: [How to confirm — test command, build, manual check]
 - Files: [Which files will be touched]

```


### 阶段 4：Implement（实现）

按照 `incremental-implementation` 和 `test-driven-development` 技能逐个执行任务。使用 `context-engineering` 在每一步加载正确的规格章节和源文件，而不是把整个规格塞给 Agent。

## 保持规格存活

规格是活文档，不是一次性产物：

- **决策变更时更新** — 如果发现数据模型需要改，先更新规格再实现。
- **范围变更时更新** — 增加或削减的功能应反映在规格中。
- **提交规格** — 规格属于版本控制，和代码一起。
- **在 PR 中引用规格** — 链回到每个 PR 实现的规格章节。

## 常见合理化

| 合理化说法 | 现实 |
| ---------------------- | -------------------------------------------------------------------- |
| "这很简单，不需要规格" | 简单任务不需要长规格，但仍然需要验收标准。两行规格就够了。 |
| "我先写代码再补规格" | 那是文档，不是规格。规格的价值在于迫使你在代码之前就清晰。 |
| "规格会拖慢我们" | 15 分钟的规格能防止几小时的返工。15 分钟的瀑布流胜过 15 小时的调试。 |
| "需求反正会变" | 所以规格是活文档。过时的规格仍然比没有规格好。 |
| "用户知道他们想要什么" | 即使清晰的请求也有隐含假设。规格暴露这些假设。 |

## 危险信号

- 没有任何书面需求就开始写代码
- 在明确"做完"是什么意思之前就问"我直接开始构建吗？"
- 实现规格或任务列表中没有提到的功能
- 做架构决策但没有记录
- 因为"很明显要做什么"就跳过规格

## 验证

在进入实现之前，确认：

- [ ] 规格覆盖了全部六个核心领域
- [ ] 人类审查并批准了规格
- [ ] 成功标准是具体且可测试的
- [ ] 边界（Always/Ask First/Never）已定义
- [ ] 规格已保存到仓库中的文件
