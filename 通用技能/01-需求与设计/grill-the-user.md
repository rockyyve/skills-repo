# Grill-the-User (Reverse Interrogation)

写代码前**让 Agent 拷问你**，强迫你把模糊需求说清楚。mattpocock/skills 中最受欢迎的两个 skill。

相关：entities/matt-pocock · entities/mattpocock-skills · references/mattpocock-skills-article · concepts/vibe-coding · concepts/ai-coding-pain-points · concepts/ubiquitous-language

## 两个变体

| Skill              | 适用场景                                   | 副作用                        |
| ------------------ | ------------------------------------------ | ----------------------------- |
| `/grill-me`        | 非代码类决策（写文档、做产品决策、定方案） | 仅生成对话纪要                |
| `/grill-with-docs` | 代码场景（改动、新功能、重构）             | **自动更新 `CONTEXT.md`/ADR** |

`/grill-with-docs` 是仓库里**使用频率最高**的 skill。建议：**每次改动前都先跑一遍，而不是直接喂 Prompt。**

## 为什么需要"反向"

常规 vibe coding 的流程是：

```
你 → 给 Prompt → Agent 直接干 → 出代码 → 才发现没对齐

```


Grill 把方向反过来：

```
你 → 触发 grill → Agent 提出 N 个问题 → 你回答 → 对齐 → 再写代码

```


效果：**把"对齐成本"前置到几分钟的对话，避免事后几小时的返工。**

## `/grill-with-docs` 的双重副作用

它不仅完成拷问，还**顺手维护两个产物**：

### 1. `CONTEXT.md`（领域语言词典）

每次澄清的术语自动 commit 进 `CONTEXT.md`。坚持几周：

- 启动 session 时不需要再向 Agent 重新定义术语。
- 整个团队（含 Agent）共享 ubiquitous language。
- token 占用立省 30%–50%。

### 2. `docs/adr/`（决策记录）

但 ADR **不是逢决策就写**，标准是：

> 决策"难以反转、缺少上下文会让人困惑、有真实 trade-off"时才写。

这条标准把 ADR 从"事后产物"变成"拷问过程的副产品"，质量自动达标。

## 在完整流水线中的位置

mattpocock/skills 推荐的"标准打法"链路：

```
/grill-with-docs ← 你在这里
 ↓
/to-prd
 ↓
/to-issues
 ↓
/tdd | /diagnose | /zoom-out
 ↓
（周期）/improve-codebase-architecture

```


Grill 是入口。**跳过它的所有"vibe coding"最终都会在后面某个阶段还回来。**

## 怎么用

```text
/grill-with-docs

[Agent 开始提问]
Q1: 这个 feature 对哪些模块有影响？
Q2: 你期望的失败模式是什么？
Q3: 有没有相关的领域术语我应该用？
...

```


回答的过程中：

- 你会发现自己有些地方根本没想清楚——好事，现在发现胜过 1 小时后发现。
- Agent 会把新术语提议加到 `CONTEXT.md`——你最后 review 一下即可。
- 如果触及到了重要决策，Agent 会建议写一个 ADR——你点头它就帮你起草。

## 反 pattern

- 直接跳过 grill 写代码——可见的短期省时间，可见的中期返工。
- 把 grill 当"AI 帮你写 PRD"——它不是替代你思考，是逼你把思考说出来。
- `CONTEXT.md` 不 review 直接 merge——会沉淀错误术语，污染未来所有 session。

## 与其他 skill 配合

- 之后 → `/to-prd` 把对话凝结成 PRD 提交 issue。
- 之前 → 没有"之前"。Grill 就是入口。
- 平行 → `/zoom-out` 当你 grill 到一半发现自己根本不懂上下文时用。

## 安装和使用

> 这是可安装的 Agent Skill，可直接用于 Claude Code、Cursor、Copilot CLI 等工具。

**Skill 名称**：`setup-matt-pocock-skills`

**本地文件**：`skills/setup-matt-pocock-skills/`（含 `SKILL.md`、`SKILL.original.md`、`USAGE.zh.md`）

### 安装方式

**方式一：复制到全局 skills 目录**（推荐，跨项目可用）

```bash
cp -r "skills/setup-matt-pocock-skills" ~/.claude/skills/

```


**方式二：复制到当前项目**

```bash
cp -r "skills/setup-matt-pocock-skills" .claude/skills/

```


**方式三：写入项目规则**

将 `skills/setup-matt-pocock-skills/SKILL.md` 内容追加到 `CLAUDE.md`、`AGENTS.md` 或 Cursor Rules。

**方式四：英文原版**

如需减少 token 消耗，使用 `skills/setup-matt-pocock-skills/SKILL.original.md` 替换 `SKILL.md`。
